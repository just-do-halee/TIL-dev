###### _by just-do-halee_

# _Today I learned_

## 2022-9-15

<br>

- Rust::Advanced

  - **_`static`_** 과 **_`const`_** 의 차이점.

    ```rust
    // 우선 static과 const는 Global scope를 포함한 모든 영역에서
    // 선언되어질 수가 있다.

    static STATIC: u8 = 42;
    const CONST: u8 = 42;

    fn main() {
        static LOCAL_STATIC: &str = "hello";
        const LOCAL_CONST: &str = "world";

        // 또한 lifetime은 자동으로 추론되며 둘 모두 변경 불가능한 immutable variable로서 선언된다.

        println!("{STATIC} == {CONST}");
        println!("{LOCAL_STATIC} {LOCAL_CONST}");
    }
    ```

    ```rust
    // 그러나 static의 경우 `mut` 키워드와 함께 변경 가능한 mutable variable로서 선언시킬 수도 있다.

    static mut STATIC: u8 = 42;

    fn main() {
        static mut LOCAL_STATIC: &str = "hello";

        unsafe {
            STATIC = 200;
            LOCAL_STATIC = "the number";
        }

        // 그러나 반드시 변경 또는 읽어내릴 시에는 `unsafe`임을 명시해야만 한다.

        unsafe {
            println!("{LOCAL_STATIC} {STATIC}");
        }
    }
    ```

  <br>

  - **_`'static`_** Lifetime 강제하기

    ```rust
    // 'static lifetime을 가진 변수들은 기본적으로
    // 프로그램이 시작되어 종료될 때까지의 lifetime을 갖는다.

    static STATIC: u8 = 42; // <- 'static
    const CONST: u8 = 42; // <- 'static

    fn main() {
        {
            let s: &str = "hello"; // <- 'static
        } // 문자열 포인터인 s는 이곳에서 drop되지만,
        // 문자열 자체인 "hello"는 binary code 내에 남아 있으므로 'static lifetime을 갖는다.

        // 그러나 문법 자체의 옵션으로서 'static 으로 선언되어진
        // 변수들을 임시로 특정 'a lifetime으로 강제시킬 수가 있다.
        {
            let a_lifetime = 42;
            let static_num_but_a_lifetime = coerce_static(&a_lifetime);
        }

        println!("{STATIC}");

        // STATIC은 여전히 'static으로 엑세스할 수 있다.
    }

    fn coerce_static<'a>(_: &'a u8) -> &'a u8 {
        &STATIC
    }
    ```

<br>

- OS::Basic

  - `부트섹터(boot sector)`: 컴퓨터에 전원이 들어오면 가장 먼저 자동으로 작동시킬 `512 bytes` 코드 블록이 입력되어 있는 기억 공간을 말한다. 하드디스크, 플로피디스크, USB 등 `어떤 기억장치이든 상관없다.` 512 bytes는 보통 운영체제(`OS`)로서 기능하고, 어셈블리어(`Assembly`)로 채워져 있으며, _그 마지막은 반드시 0x55 0xaa (= 0101 0101 1010 1010) 으로 채워져 있어야 한다._

  - 컴퓨터가 켜지면, 부트섹터로부터 512 bytes의 운영체제 코드를 RAM상에 올린다. 이 메모리 공간을 `OS space`라 부르고, 남은 공간을 `User space`라 부르며 여러 방식으로 할당, 해제하며 사용한다.

  - `CPU`는 제어부(Control Unit: `CU`)와 산술논리장치(Arithmetic and Logic Unit: `ACL`) 두 부분으로 나뉘며 실제 수학적인 연산 전체를 담당하는 것은 `ACL`이 된다. 이는 조합논리회로로서 여러 레지스터들이 함께 조합되어 작동한다.

  - 운영체제(OS)가 하는 일은, `무엇을 어디에, 얼마만큼 할당할 것인가`를 결정하는 것이다.

  - 하드디스크에 존재하는 0과 1의 Binary 집합 코드, 프로그램을 이용자가 실행할 시에, 운영체제(OS)는 그 프로그램 코드를 복사하여 RAM상에 올리게 된다.

  - RAM은 하드디스크보다는 CPU에 직접적으로 연계되어 훨씬 빠른 데이터 송수신이 가능하기에, 실행(CPU의 레지스터들에 넣고 가동)시킬 프로그램은 RAM상에 올리는 방식을 채택한다. 다만 RAM은 컴퓨터 전원이 나가면 데이터 소실이 일어나기에 이어 보관해야 할 데이터들은 다시금 하드디스크로 재전송하는 과정을 필요로 한다.

  - 프로그램 자체는 하드디스크에 여전히 두고, 그 복사본을 따서 RAM에 올리며 여러 메타데이터들을 추가해 하나의 데이터 구조체를 만드는데, 이를 `프로세스`라 한다.

  - 하나의 프로그램을 여러 번 실행시킬 시, 동일한 프로그램 코드를 지녔지만 여러 개의 프로세스를 생성해낼 수 있다.

  - 프로세스의 구조:

    1.  메모리 구조 [프로세스 제어 블록(Process Control Block: `PCB`)]

        - `Stack`: 함수 인수들을 임시적으로 저장한 후, 함수 호출 후에 다시 꺼내 쓴다든가 함수 내에 선언되어 계산된 직후 함수의 결과값이 되기 전까지만 사용되는 등 임시 계산용 변수들을 잠깐씩 넣고 빼고 연산할 때 필요한 일종의 기억 자료구조. Stack Memory의 크기는 컴파일 시에 계산되어 바이너리 코드에 기입되며, 프로그램 진행 시 함수 호출 시마다 기입된 양의 메모리를 할당 받고, 함수 호출이 종료될 시 다시 자동으로 해제되는 등 프로그래머가 일일이 신경 써서 할당하지 않아도 되게끔 고안된 메모리 구조이기도 하다.

        - `Heap`: 사용자(IO Device)에 의해 언제든지, 얼마만큼 메모리 공간이 필요로 할지 알 수 없기에 그때 그때 할당해서 사용하게끔 고안된 메모리 영역. 프로그래머에 의해 자유롭게 할당 및 해제할 수 있으며 실시간으로 변동 중인 RAM의 영역을 잘 정리 및 계산하여 할당해야 하기에 `미리 정해 놓고 할당 및 해제하는 Stack`보다는 한참 퍼포먼스가 부족하다.

        - `Data Part`(Static|Global Variables): 프로세스 생성 시 PCB 내에 정적, 전역 변수들을 바로 할당하고 사용하기 위함이다.

        - `Program`: 원본 프로그램의 Binary Code.

    2.  자료 구조 [프로세스 속성(Process Attributes)]

            a. `Process Id (PID)`: 각 프로세스별 할당되는 고유한 번호(숫자)
            b. `Program Counter`: CPU 내부에 있는 레지스터 중 하나로, 다음 실행 될 코드의 위치 주소를 갖고 있다.
            c. `Process State`: 프로세스의 현재 상태
            d. `General Purpose Registers`: 범용 레지스터. 프로세스가 시행 중 다양한 데이터들을 저장해야 하는데, 그 값들이 저장되는 CPU 내부에 존재하는 레지스터들을 의미한다.
            e. `Priority`: CPU 연산 할당 기준의 우선도.
            f. `List of Open Files`: 프로세스가 연(open) 파일들의 리스트를 보관한다.
            g. `List of Open Devices`: 프로세스가 연(open) 장치들의 리스트를 보관한다.
            h. `Protection`: A프로세스가 사용 중인 Stack과 Heap 등의 메모리 공간을 B프로세스가 마음대로 접근해서 이용해서는 곤란하기에 일종의 프로텍션이 필요하다.

<br>

- ETC

  - 펌웨어(`Firmware`)란, 하드웨어 단계에서 소프트웨어를 조작 가능하게 하는 일종의 선입력된 기계부품식 프로그램을 뜻한다.
  - `BIOS`(Basic Input/Output System)이란, IO Device와 CPU와의 브릿지 역할을 하는 가장 기초적인 펌웨어이다. 컴퓨터를 켜면 OS와 IO 사이의 데이터 흐름을 관리한다.
  - 현대에는 `UEFI`(Unified Extensible Firmware Interface)를 표준으로 사용한다. 인텔이 개발한 EFI 규격에서 출발했으며 BIOS를 대체할 목적으로 탄생하였다. 현대에는 더 이상 BIOS를 사용하지 않는다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
