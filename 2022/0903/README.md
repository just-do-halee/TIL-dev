###### _by just-do-halee_

# _Today I learned_

## 2022-9-3

<br>

- Mindset

  - 좋은 팀과 좋은 사람만이, 좋은 제품을 만들 수 있다.

<br>

- Rust::Toolchain

  - ::Target

    ```bash
    $ rustup target list
    ```

    ```
    aarch64-apple-darwin (installed)
    aarch64-apple-ios
    aarch64-linux-android
    aarch64-pc-windows-msvc
    aarch64-unknown-linux-gnu
    arm-linux-androideabi
    arm-unknown-linux-gnueabi
    ...
    ```

    ```bash
    $ rustup target install ...
    ```

    ```bash
    $ cargo build --target ...
    ```

    - CPU 제품에 부합하는 프로그램 코드(바이너리)를 타겟하여 컴파일한다. 또는 바이너리가 아닌 이를테면 WASM(Web Assembly) 바이트 코드를 타겟하여 컴파일 또한 가능하다.

  <br>

  - ::ASM

    ```bash
    $ RUSTFLAGS="--emit asm" cargo build
    ```

    - target 폴더 내에 .s(어셈블리로 변환된) 파일을 내보내며 컴파일한다.

    <br>

    ```bash
    $ cargo install cargo-show-asm
    ```

    - 어셈블리를 보다 보기 좋고 깔끔하게 보여주는 툴이다. [cargo-show-asm](https://crates.io/crates/cargo-show-asm)

      ##### _example_)

      ```bash
      $ cargo asm PACKAGE_NAME::main --dev --target ... --rust
      ```

      - 사실 https://godbolt.org/ 더 좋은 사이트도 있다.

<br>

- ::Hardware

  - `Apple Silicon`은 ARM Holdings에서 개발한 CPU 아키텍처인 `ARM` 자체의 라이센스를 취득 독자적인 ARM기반 CPU 아키텍처를 설계했다.

  - `Darwin`은 Apple 제품들에 사용되는 운영체제(OS)이다. 오픈소스이다.

  - Apple Silicon의 `aarch64-apple-darwin` 사양에서 Intel칩인 `x86_64-apple-darwin`이 실행될 수 있다.

<br>

- OS::Basic

  - ::Time

    - **_`Point in Time`_**: 시간의 한 지점.

    - **_`Duration in Time`_**: 두 지점(Point in Time) 사이의 거리.

      ##### example)

      ```
        1 PM - 3 PM
      ```

      - 한 사원이 오피스에 1 PM에 출근했고, 2 Hours 일한 뒤 3 PM에 퇴근했다고 한다면, 1 PM과 3 PM은 각 Point in Time이라 부를 수 있고 그 사이 걸린 시간 2 Hours는 Duration in Time이라고 할 수 있다.

    <br>

    - 프로세스와 관계된 다양한 시간들(Various times related to a process)

      1. `Arrival time: AT`: 프로세스가 생성되어 RAM으로 옮겨진 딱 그 시간(Point in Time). 프로세스가 [Ready]상태(=waiting, idle)로 존재한다.
      2. `Burst time(=Execution time): BT`: 프로세스가 CPU에 의해 실질적으로 연산되어 진전된 만큼의 시간(Duration in Time).
      3. `Completion time: CT`: 프로세스가 종료되어 RAM으로부터 완전히 지워지기 직전, 프로세스가 [Terminated]상태가 된 딱 그 순간의 시간(Point in Time).
      4. `Turnaround time: TAT`: 프로세스가 RAM으로 옮겨진 후부터 (모두 시행되어) 다시 RAM으로부터 완전히 삭제된 시점까지의 소요 시간. 프로세스가 RAM 내에 존재했던 만큼의 시간(Duration in Time). (= **_completion time - arrival time_** = _burst time + i/o time + waiting time_)
      5. `Waiting time`: 프로세스가 RAM상에 존재하나 CPU에 의해 연산이 되지도, I/O Device에 의해 읽거나 쓰여지지도 않고 있는 순수한 대기 중인 시간(Duration in Time). 리소스 부족이 원인이다.
      6. `Response time`: ...
      7. `I/O time`: 입출력 장치와 소통 중인 만큼의 시간(Duration in Time).

  <br>

  - ::Scheduler

    - CPU에 관한 스케쥴링 알고리즘(Scheduling Algorithms)은 크게 두 가지로 나뉜다.

      1. Preemptive Scheduling Algorithm: CPU에 할당된 프로세스의 우선도(Priority)를 넘어서는 비교적 더 높은 우선도를 지닌 프로세스가 RAM상에 [Ready]상태로 존재할 시, 둘을 스왑해 우선도가 더 높은 프로세스를 선제적으로 처리하는 알고리즘을 말한다.
      2. Non-preemptive Scheduling Algorithm: 한 번 CPU에 할당된 프로세스는 Completion이 될 때까지 선제적으로 스왑 당하지 않는다. 완전히 마쳐진 후에야 다시금 RAM 내의 [Ready]상태 프로세스들을 쭉 훑고 가장 높은 우선도를 지닌 프로세스를 CPU에 적재해 시행시키는 알고리즘을 말한다.

    <br>

    - ::참고::

      - [`I/O`]상태에 있는 프로세스들은 스케쥴러에 의해 CPU에 할당된다고 하여도, 어차피 I/O Device로부터 읽거나 쓰는 시행이 종료되지 않는다면 그 다음 코드를 CPU가 연산할 수 없으므로, 즉 [`Block`]되므로 애초부터 스케쥴링 알고리즘은 [`I/O`]상태 프로세스를 스케쥴링 대상으로 고려하지 않는다.
        - [`I/O`]상태를 [`Blocked`]상태라고 부르는 이유이기도 하다.

    <br>

    - 잘 알려진 스케쥴링 알고리즘들

      - 최단 작업 우선 스케쥴링 알고리즘(Shortest Job First Scheduling Algorithm: `SJF Scheduling`): 가장 Burst time(=Execution time)이 짧은 것을 우선(Priority)으로 할당한다.

        1. Non-preemptive Scheduling Algorithm

           ##### example)

           ```js
             // PID: Process Id
             // AT : Arrival Time
             // BT : Burst Time

             | PID | AT | BT |
                1    2    3
                2    3    2
                3    4    3
                4    6    1
                5    8    2

             // * IO는 없다고 가정한다.
           ```

           ```bash
                  0 .. 2 .... 5
            실행중       ^--P1--^
            대기중        P2, P3
           ```

           [`1`] P1이 먼저 도착하며 Non-preemptive이기에 한텀에 끝까지 완료된다. Point in Time 3에 P2가, 4에 P3가 도착하나, P1이 완료될 때까지 대기해야 한다.

           ```bash
                  0 .. 2 .... 5
            실행중       ^--P1--^
            대기중        P2, P3
           ```

           [`2`] 총 실행 소요시간(BT)가 2인 P2와 3인 P3 중, 소요 시간이 가장 짧은 P2를 그 다음 연산 대상으로 지목해 할당한다.

           ```bash
                  0 .. 2 .... 5 .... 7
            실행중              ^--P2--^
            대기중           P3, P4
           ```

           [`3`] 다음으로는 남은 P3(3)와 도중에 들어온 P4(1) 중 골라낸다. P4가 훨씬 작기에 간택된다.

           ```bash
                  0 .. 2 .... 5 .... 7 .. 8
            실행중                     ^-P4-^
            대기중            P3,           P5
           ```

           [`4`] 이번에는 P3(3)와 P5(2)다.

           ```bash
                  0 .. 2 .... 5 .... 7 .. 8 .. 10
            실행중                          ^-P5-^
            대기중            P3
           ```

           [`5`] 마지막으로 P3가 그제야 혼자 남았다.

           ```bash
                  0 .. 2 .... 5 .... 7 .. 8 .. 10 ... 13
            실행중                               ^--P3--^
            대기중
           ```

           [`6`] 모든 프로세스가 완료된다.

           ```js
             // PID: Process Id 프로세스 아이디
             // CT : Completion Time 연산 완료 된 시간
             // TAT : Turn-around Time 최종 소요 시간
             //       = CT - AT (완료시간 - 도착시간)
             //       = WT + BT + IOT (대기 + 실행 + IO)
             // WT : Waiting Time 대기 시간
             //       = TAT - BT - IOT

             | PID | CT | TAT | WT |
                1    5     3    0
                2    7     4    2
                3    13    9    6
                4    8     2    1
                5    10    2    0

             // Average TAT : (3 + 4 + 9 + 2 + 2) / 5
             //               = 4
             // Average WT : (0 + 2 + 6 + 1 + 0) / 5
             //               = 1.8
           ```

           [`7`] 최종적으로 `최종평균소요시간`(Average Turn-around Time) 및 `최종평균대기시간`(Average Waiting Time)을 구할 수 있게 된다. (한 프로세스 당 처리 완료까지 평균 얼마만큼의 시간이 소요되는지, 평균 얼마만큼의 대기 시간이 필요할지)

        2. preemptive Scheduling Algorithm

           - ...

    <br>

    - ::참고::

      - **_`Schedule Length`_** (스케쥴 길이):

        - 마지막 프로세스의 완료 시점 - 첫 프로세스의 도착 시점
        - (Completion time of last process - Arrival time of first process)

      - **_`Throughput`_** (처리량):

        - 완료된 프로세스 개수 / 스케쥴 길이
        - (Number of processes / Schedule Length)
        - 스케쥴 길이 내에 얼마만큼의 프로세스들이 완료되었는지
        - 단위 시간(스케쥴 길이) 당 완료된 프로세스의 수

  <br>

  - ::ETC

    - Hard Disk건, USB이건 stream을 통해 데이터를 주고받는 모든 것들을 I/O Device라고 부를 수 있다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
