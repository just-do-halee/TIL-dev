###### _by just-do-halee_

# _Today I learned_

## 2022-9-1

<br>

- Rust::Lifetime

  - **_비어휘적 생명주기들 (Non-lexical lifetimes: `NLL`)_**

    - 2018edition부터 추가되었기는 하나, 이번 1.63부터 완전히 stable default로서 활성화 되었다.

    ```rust
        fn main() {
            let mut v = vec![1, 2, 3, 4];
            let slice = &v[0..2]; // reference borrowing
            v.push(1); // mutable borrowing
        }
    ```

    - 이전이라면 컴파일되지 않을 깐깐한 lifetime checking이 보다 인간적으로 해석되어 불필요한 학습 곡선의 장벽을 굉장히 낮추게 되었다. 즉, 컴파일러가 보다 똑똑해졌다.

    ```rust
        fn main() {
            let mut v = vec![1, 2, 3, 4, 5, 6];
            let i = &mut v[0]; // mutable borrow occured
            if i == &mut 2 {
                *i = 32; // still mutable borrow occured in the scope
                v.push(2); // another mutable borrow occured
            }
        }
    ```

<br>

- OS::Basic

  - ::CPU

    - **_`CPU`_** 는 대략 제어부(Control Unit: `CU`)와 산술논리장치(Arithmetic and Logic Unit: `ACL`) 두 부분으로 구성되어 있다.

    - 산술논리장치(ACL)는 `조합논리회로`로서 실제 CPU의 수학적인 연산 전체를 담당한다.

    <br>

  - ::Program

    - 프로그램은 세 가지 상태를 지닌 채 `RAM` 상에 존재할 수 있다.

      1. Execution: CPU가 연산을 진행 중인 말그대로 `실행 중`인 상태이다.
      2. I/O event: Input/Output 장치로부터 `전달 및 전달 받기가 진행 중`인 상태이다.
      3. Waiting: 위와 같은 두 상태조차 아닌, 프로그램 자체가 `완전히 멈춘 채 기다리는` 상태를 말한다.

    - **_`Turnaround Time`_**(소요 시간)이란, 프로그램의 세 가지 상태 모두 관계 없이 프로그램이 시작되어 종료됐을 때까지의 총 소모 된 전체 시간을 일컫는다.

      ```py
      Turnaround Time = Waiting Time + Burst(Execution) Time + I/O Time
      ```

    - 프로그램을 더블클릭할 경우,

      1. Hard Disk에 존재하는 프로그램의 Machine Code가 복사, 붙여넣기 되어 새로운 인스턴스(Instance)로서 탄생한다. 이를 프로세스(`process`)라고 부른다.
         - 이때 프로세스는 [`New`]상태가 된다.
      2. 만들어진 프로세스는 RAM으로 옮겨진다. 옮겨진 프로세스는 Execution도, I/O event도 아닌 Waiting인 채로 정지해 있는다.
         - 이때 프로세스는 [`Ready`]상태가 된다.
      3. CPU에 의해 한 묶음 한 묶음 바이너리 코드가 읽혀지고, 말그대로 실행이 되어진다.
         - 이때 프로세스는 [`Running`]상태가 된다.
      4. 입출력장치로부터 데이터를 전송 받든가 전송한다.
         - 이때 프로세스는 [`I/O(or Block)`]상태가 된다.
      5. 모든 바이너리 코드가 읽혀지고(=프로세스가 완전히 종료되고) RAM으로부터 Machine Code를 완전히 소멸시킨다.
         - 이때 프로세스는 [`Terminated`]상태가 된다.
      6. ............
         - 이때 프로세스는 [`Suspend Ready`]상태가 된다.
      7. ............
         - 이때 프로세스는 [`Suspend Wait`]상태가 된다.

    - 한 프로그램을 여러 번 더블클릭할 경우 동일한 원본으로부터 여러 프로세스(process)가 동시에 생성된다.

    <br>

  - ::IO

    - 입출력장치(I/O Device)는 전부 개개의 Buffer(일종의 RAM)을 갖고 있으며 이를테면

      - 입력장치 중 하나인 키보드는 키(Key)를 눌렀을 때, 그 키에 해당하는(의미하는) 바이너리 데이터를 자가 Buffer에 덮어씌워 놓고, 연결된 컴퓨터의 RAM으로 전송해 붙여넣는 작업을 한다.

      - 출력장치 중 하나인 모니터는 연결된 컴퓨터의 RAM으로부터 바이너리 데이터를 전송 받아, 모니터 자가 Buffer에 붙여넣는다.

    <br>

  - ::ETC

    - 멀티프로그래밍의 정도(Degree of multiprogramming): RAM(..)이 최대로 수용할 수 있는 단일 프로세스의 최대 개수.

    - 정보의 단위

      ```js
          1 bit = 0 or 1

          1 byte = 8 bits

          1 KiB = 2^10 bytes // 키비바이트
          1 MiB = 2^20 bytes // 메비바이트
          1 GiB = 2^30 bytes // 기비바이트
          1 TiB = 2^40 bytes // 테비바이트
          1 PiB = 2^50 bytes // 페비바이트
          1 EiB = 2^60 bytes // 엑스비바이트
          1 ZiB = 2^70 bytes // 제비바이트
          1 YiB = 2^80 bytes // 요비바이트
      ```

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
