###### _by just-do-halee_

# _Today I learned_

## 2022-9-2

<br>

- OS::Basic

  - ::OSType // 운영체제(Operating System)의 종류

    1. **_`Batch OS`_**(Single CPU): 멀티프로그래밍의 정도(Degree of multiprogramming)가 1인 운영체제. 한 타임에 단 하나의 프로세스만 수용한다.

       - 이제는 사용되지 않는다. 단 하나가 아닌, 동시에 여러 프로세스를 수용하는 OS의 이점이 크기 때문이다. 한 프로세스가 CPU에 의해 실행되어지고 있는 동안, 다른 한 프로세스는 I/O event를 진행하고 있을 수가 있기 때문이다. 즉, 병렬적 실행이 가능하다.

    2. **_`Multiprogramming OS`_**(Single CPU & Concurrent Processing): RAM 내에 여러 프로세스를 동시에 수용하는 운영체제.

       - CPU에 의해 실행 되지 않고 있는 프로세스들이 I/O event를 진행할 수 있으며 CPU는 쉬지 않고(not idle) 일할 수 있다. 즉, CPU 효율성이 개선 된다.

    3. **_`Multiprocessing OS`_**(Multi CPU & Parallel Processing): 둘 이상의 CPU를 핸들링할 수 있는 운영체제. 둘 이상의 CPU가 병렬적으로 처리되어 고성능을 꾀할 수 있다.

    <br>

    - ::참고::

      - CPU 효율성(`CPU efficiency`): _CPU가 실제 사용된 시간(Useful time of CPU) / 전체 프로세스가 시작되어 종료될 때까지의 시간(Total process run time)_

        - CPU 효율성이 극대화될수록 컴퓨터의 속도는 빨라진다고 할 수 있다.

      - 병렬처리(`Parallel` Processing) vs 동시처리(`Concurrent` Processing):

        - 병렬처리는 실제 처리자(Processor)가 여러 개 존재하여 각기 동시에 리소스들을 처리하는 것을 일컫는다.
        - 동시처리는 단 하나의 처리자(Processor)가 여러 리소스들을 처리하되, 각 리소스들이 내부적으로 처리되고 있는 시간대에 단지 기다리기만 않고 즉시 대기 중인 리소스를 처리하는 효율적인 알고리즘을 일컫는다.

  <br>

  - ::Process

    - 수동 객체(`Passive Entity`): Hard Disk에 존재하는 프로그램의 원본 바이너리 파일.
    - 능동 객체(`Active Entity`): 원본 프로그램으로부터 생성된 프로그램 프로세서들.

    - 프로세스 제어 블록(Process Control Block: `PCB`): 한 프로세스를 이루는 개체들의 모음이라고 봐도 좋다. 한 프로세스는 반드시 한 PCB를 갖는다. 각 파트는 전부 메모리 일부에 할당되어 존재한다.

      - `Stack`: 프로그램의 기능들을 수행할 적에, 겹겹이 여러 기능들이 여러 기능 안에 포함되어 있을 시 내부 기능을 시행한 후 다시 외부 기능으로 돌아와 이어서 시행해야 함에 있어 순차적인 코드의 나열을 기억하기 위한 일종의 기억 자료구조를 일컫는다.
      - `Heap`: 프로세스가 수행되고 있는 그 도중(runtime)에 메모리 공간 일부를 사용하기 위해 할당(점유)(ex:) malloc, calloc, ..)하는 메모리 풀을 다른 말로 풀어쓴다.
      - `Data Part`(Static|Global Variables): 정적, 전역 변수들은 프로세스 생성 시에 PCB 내에 바로 할당되어 사용되어진다.
      - `Program`: 원본 프로그램의 Binary Code.

    <br>

    - 프로세스 속성(Process Attributes): 프로세스들은 각기 이 속성들을 지닌다. 모든 속성들은 CPU 할당 스위칭에 있어 필요한 정보들을 보관한다(시행 도중 잠시 다른 프로세스로 대체되었다가 돌아왔을 때 이어 시행될 수 있도록 저장/불러오는 정보들 포함).

      1. Process Id (PID): 각 프로세스별 할당되는 고유한 번호(숫자)
      2. Program Counter: CPU 내부에 있는 레지스터 중 하나로, 다음 실행 될 코드의 위치 주소를 갖고 있다.
      3. Process State
      4. General Purpose Registers: 범용 레지스터. 프로세스가 시행 중 다양한 데이터들을 저장해야 하는데, 그 값들이 저장되는 CPU 내부에 존재하는 레지스터들을 의미한다.
      5. Priority: CPU 연산 할당 기준의 우선도.
      6. List of Open Files: 프로세스가 연(open) 파일들의 리스트를 보관한다.
      7. List of Open Devices: 프로세스가 연(open) 장치들의 리스트를 보관한다.
      8. Protection: A프로세스가 사용 중인 Stack과 Heap 등의 메모리 공간을 B프로세스가 마음대로 접근해서 이용해서는 곤란하기에 일종의 프로텍션이 필요하다.

    <br>

    - `Context`: 프로세스 제어 블록(Process Control Block: PCB)과 프로세스 속성들(Process Attributes)을 통틀어 프로세스의 컨텍스트라고 부른다.

    <br>

    - ::참고::

      - Stack과 Heap은 따로 프로세스별 한계가 정해져 있지 않다. 실시간으로 프로세스별 Stack과 Heap의 크기가 유동적으로 바뀐다.

      - 지역 변수(Local Variables)는 Stack에 저장된다.

  <br>

  - ::Scheduler // OS code의 일부로서 존재하는 기능. 둘 이상의 프로세스가 존재할 경우 어떤 프로세스를 우선적으로 CPU에 할당시킬지 일정을 짜맞추는 일을 한다.

    - Scheduler의 종류:

      1. Long Term Scheduler
         - Hard Disk로부터 생성된 [New]상태의 프로세스들 중 어떤 것을 우선적으로 RAM 내에 할당시킬지 그 일정을 조율하는 스케쥴러.
      2. Short Term Scheduler
         - RAM에 올라간 프로세스들 중 어떤 것을 먼저 CPU 연산에 할당시킬시 일정을 조율하는 스케쥴러.
      3. Medium Term Scheduler
         - RAM상에 존재하는 프로세스들과 Hard Disk상에 생성되어 대기중인 프로세스들의 각 우선도를 살펴보고 서로 스왑(바톤 터치)시켜 더 높은 우선도의 프로세스를 우선적으로 시행시키는 일정을 조율하는 스케쥴러.
           - `Swapping`: Swap-In / Swap-Out

    - Scheduling Algorithm 종류:

      1. First come first serve
         - 먼저 RAM에 할당된 프로세스 우선으로 처리한다.
      2. Shortest job first
         - 소모 시간이 가장 짧은 프로세스 우선으로 처리한다. 만일 A프로세스가 처리중에 있어서 현재 A프로세스의 종료까지 남은 시간이, 새로 들어온 B프로세스의 소모 시간보다 크다면, A프로세스를 중단하고 즉각 B프로세스를 CPU에 할당해 실행시킨다.

    <br>

    - ::참고::

      - **_`Context Switching`_**: Short Term Scheduler에 의해 우선도(Priority)가 더 높은 프로세스를 CPU에 할당하는 그 시점에, 그 이전까지 실행 중이던 비교적 우선도가 낮은 그 프로세스를 일시간 보류시키는 과정에서, 서로의 _Context(PCB + Attributes)_ 를 스왑하는 상황을 일컫는다.

  <br>

  - ::Thread // 한 프로세스 내에 존재하는 작업 흐름 및 단위를 말한다. 기본적으로 Main Thread가 있으며 단 하나의 흐름을 갖는다. 그러나 프로세스는 다수의 Thread를 가질 수 있다.

    - 멀티 스레딩(Multi-threading): 한 프로세스가 다수의 Thread를 갖는 것을 말한다. 각 Thread는 각자 고유의 Stack을 지니며 연산 흐름을 진행한다. 그러나 그 외 Heap, Data Part(static/global variables), Program 영역은 공유하고 또 접근해 조작할 수도 있다.

      - _왜 멀티 스레딩을 사용하는가?_ 프로그램을 두 개 실행해 프로세스를 통으로 하나 더 만드는 것보다, 한 프로세스 안에서 다만 쓰레드를 두 개 만드는 것이 훨씬 싸기(Cheap) 때문이다. 덤으로 각 Thread는 한 프로세스 내의 할당된 자원들을 공유할 수 있다.

    - 새로운 스레드를 만드는 것은 평균 기본적으로(OS마다, 설정마다 다르겠지만) 2메가바이트의 메모리 할당이 요해진다. 새 Stack 영역을 생성하는 것이다.

    - CPU가 A스레드에서 B스레드로 스위칭할 적마다 꽤 비싼(Expensive) 컨텍스트 스위칭(Context Switching)이 일어난다. 고로 한 CPU 내에서 보다 많은 스레드를 사용하려고 하면 할수록 크나큰 오버헤드를 치루어야 할 것이다.

      - 한 프로세스 내에서 더욱이 작은 작업들로 코드를 쪼개어 시행하기에 CPU는 프로세스 간의 컨텍스트 스위칭보다도 더 잦은 컨텍스트 스위칭을 발생시켜야 하기 때문이다. 물론 CPU 코어 수가 많다면 이는 또 다른 이야기가 되겠다.

  <br>

  - ::ETC

    - 운영체제(OS)의 기본 기능이란, "**_무엇을 어디에, 얼마만큼 할당할 것인가_**"를 결정하는 것이다.

    - 오버헤드(Overhead)는 어떤 처리를 하기 위해 들어가는 간접적인 처리 시간, 메모리 등을 말한다.

    - 비동기(Asynchronous)란, CPU 컨텍스트 스위칭을 하는 운영체제(OS)의 손길이 닿는 멀티 스레딩(Multi-threading)과는 다르게 코드 내에서 일(Task)처리를 조율해 프로세스 Blocking(Idle 상태) 없이 프로세스를 시행하는 프로그래밍 방식을 말한다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
