###### _by just-do-halee_

# _Today I learned_

## 2022-9-4

<br>

- Rust::Thread

  - ::Channel // 스레드간 소통하기

    - Standard Library

      ```rust
      // multi-producer, single-consumer
      use std::sync::mpsc;
      ```

    - Firefox의 일부분인 Servo엔진에 사용되기 위하여 최초 러스트 코어팀에 의해 개발되었다. 그러나 이후 Standard Library에 포함된 뒤, 그 디자인 선택에 실수가 있었음을 깨닫고 어떻게 해보려 하였으나, 호환성을 문제에 봉착해 포기할 수밖에 없었다.

    - 그리하여 아예 새로운 라이브러리에 디자인을 새로 만들기로 하였고, 그것이 [crossbeam crate](https://crates.io/crates/crossbeam)의 *crossbeam::channel*이다.

      - 보다 빠르며, 보다 효율적이고, 보다 많은 기능들이 있다.

    <br>

    - **_`Channel`_** 이란, 스레드들이 접근할 수 있는 단방향 큐(Queue)를 말한다. *Channel*은 수신자와 송신자가 서로 데이터를 주고받을 수 있으며, 그 데이터는 강력하게 Typed되어 있다.

      - 그 Type은 반드시 Send Trait이 적용돼 있어야 한다. Send Trait은 Copy Trait과 마찬가지로 컴파일러가 알아서 _스레드간에 주고받아도 안전하다고 판단된다면_ 적용시킨다. ex:) 모든 primitive type들 및 대부분의 standard library type들은 적용돼 있다.

      - 직접 Send Trait을 적용시킬 수 있긴 하나, 정말 많은 경우 컴파일러의 의도를 따르는 것이 좋으므로, 억지로 Send를 구현하기보다 전송 자체의 디자인을 새로 생각해보는 것이 옳을 것이다.

    <br>

    - 종류

      1. channel::`bounded`(n)

         - n만큼의 고정된 용량을 지닌다. 만일 채널 큐(Queue)가 가득 차 있음에도 송신측 스레드가 데이터를 송신하고자 한다면, Blocking된다. 수신측 스레드로부터 채널 큐의 데이터를 당겨 가져갔을 때에야 송신측 스레드는 다시 시행된다.

         - 수신측의 데이터 처리량이 송신측의 데이터 전송량을 견뎌낼 수 없다면 당연하게도 송신측 스레드는 지속적인 병목 현상을 겪을 수밖에 없다. 마찬가지로 송신측 처리량이 수신측 처리량에 훨씬 미치지 못한다면 수신측 처리량이 대기하는 시간이 길어질 수밖에 없으므로 자원의 낭비가 발생할 수도 있다. 물론 이와 관련해서는 async rust로 해결할 수 있겠다.

      2. channel::`unbounded`()

         - 메모리 초과 또는 충돌될 경우가 아닌 이상에 제한 없는 용량을 지닌 채 언제든지 송수신할 수 있다. 메모리 상황만 잘 견뎌 내는 선에서 대용량 스트림이 요구된다면 적합한 채널이 될 수도 있다.

      <br>

      - ::참고::

        - bounded, unbounded 모두 다수의 수신자를 지닐 수 있다. 다만 송신자로부터 받는 데이터, 즉 채널 큐로부터 Pop하는 그 순간은 단 한 수신자뿐이 불가능하다. 채널 큐를 동시에 조작할 수는 없다. ([crossbeam](https://crates.io/crates/crossbeam) channel)

        - bounded, unbounded 모두 다수의 송신자를 지닐 수 있다. 다만 채널 큐는 하나이며 동시에 조작할 수는 없기에, 먼저 전송되어야 먼저 수신될 수 있다.

        - bounded, unbounded 모두 다수의 송신자와 다수의 수신자 모두 동시에 지닐 수 있다. 채널 큐는 위와 같다. ([crossbeam](https://crates.io/crates/crossbeam) channel)

        - 만일 양방향 통신이 가능하게 하고 싶다면, 두 개 이상의 채널을 사용하여 한 스레드가 송신도, 수신도 모두 가능하게 하면 된다.

          - 그러나 만일 채널의 흐름이 순환적일 수 있고, 그렇다면 잠재적으로 `Deadlock` 상태에 빠질 수도 있기에 철저히 검토해 보아야 한다.

            ##### example)

            ```bash
                     | -> [2, 2, 2, 2, 2, 2] -> |
            Thread A                              Thread B
                     | <- [4, 4, 4, 4, 4, 4] <- |
            ```

            - 두 개의 송수신 bounded(6) 채널이 존재할 경우, 그리고 서로 충분한 빠르기의 처리량을 보유할 경우, 이와 같은 상황에 부닥쳐 둘 모두 Blocking 상태에 빠질 수가 있다.

            - 그 외에도 여러 불리한 가능성이 존재하므로 최대한 스레드간 통신은 단방향적, 비순환적 그래프로 이루는 편이 안전하다.

    <br>

  - ::ETC

    - Main Thread가 종료되는 순간 _Child Thread_ 또한 모두 강제 종료된다. 고로 온전히 끝내야 하는 *Child Thread*가 존재한다면 Main Thread 종료 직전 `join()` 메소드로 *Child Thread*의 완료를 기다려야 한다.

    - `std::thread::spawn()`은, 운영체제(OS)가 직접 구성하고 생성하는 OS Thread이다.

    - `Send` Trait은, Copy나 Eq와 같은 Marker Trait이다.

<br>

- OS::Basic

  - ::Process

    - **_`Context Switching`_**

      - PCB(Process Control Block)과 PA(Process Attributes) 전체를 `Context`라 부르며,

      - Process Attributes 내에

        1. Process Id (PID)
        2. Program Counter
        3. Process State
        4. General Purpose Registers
        5. Priority
        6. List of Open Files
        7. List of Open Devices
        8. Protection

      - 이 값들이 보관되어 있는데, CPU가 연산 중이던 프로세스를 잠시 보류하고 새로운 프로세스를 받아들일 때에 가장 대표적으로 CPU 내에 연산을 위해 존재하던 연산용 레지스트리(메모리)에 저장되어 있던 값들을 연산하던 프로세스의 PA 내에 옮겨 놓고, 새로 들어온 프로세스의 값들로 레지스트리를 덮어씌우는 작업을 해야 하는데, 이를 컨텍스트 스위칭(Context Switching)이라 한다.

      <br>

      - ::참고::

        - 이 CPU 레지스트리 스위칭의 경우 Thread간 변동에도 발생하므로 `Thread`에서도 *Context Switching*이라는 용어를 자주 쓴다.

    <br>

  - ::Thread

    - **_`sleep()`_**

      - _sleep(..)_ 함수는 해당 스레드를 CPU로부터 벗어나게 하며, 스레드 스케쥴러(Thread Scheduler)의 관리 목록으로부터 제외시킨다. 스레드의 상태를 [Running]상태에서 [Suspend]상태로 바꿈과 동시에 스케쥴링의 대상이 아니게 된다.

      - `sleep(1ms)`: 인자로 1ms를 넣을 경우 실제 1밀리세컨드 동안 쉬게 되는 것이 아니다. CPU로부터 벗어나게 하는 시간, 상태를 Suspend로 바꾸는 시간, 그리고 1ms 이후에 다시 CPU에 할당하고 Running 상태로 변경하는 시간 등을 모두 포함하면 실제로는 1ms 이상을 쉬는 상황이 발생하게 되어 있다.

      - CPU 할당과 비할당, 상태 전환 등은 모두 당시(그 순간)의 운영체제(OS) 및 하드웨어 상태에 따르기에 반드시 `무작위성`을 내포할 수밖에 없다.

        - 이를 토대로 랜덤 수를 만들어낼 수 있겠으나, 만일 운영체제의 상태를 일정하게 유지할 수만 있다면 무작위성은 사라질 가능성이 있기에 여러 가지를 따져 보아야 할 것이다.

<br>

- Tools

  - [Hyperfine](https://github.com/sharkdp/hyperfine): A command-line benchmarking tool.

    ##### example)

    ```bash
    hyperfine "bun hello.js" "node hello.js" "deno run hello.js" --warmup=100
    ```

<br>

- ETC

  - Async-Await-IO in Single Thread: IO를 시작해놓고, loop을 돌면서 IO가 종료되었는지 안 되었는지 수시로 확인함과 동시에 다른 코드들을 진행한다. 이런 식으로 하면 프로세스와 스레드는 [I/O]상태로서 Blocking되지 않고 [Running]상태를 유지하며 프로그램을 진전시킬 수 있다.

  - 시간 단위

    ```js
      // s = 세컨드(초)

      1 s = 1000 ms // 밀리세컨드
      1 ms = 1000 µs // 마이크로세컨드
      1 µs = 1000 ns // 나노세컨드
    ```

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
