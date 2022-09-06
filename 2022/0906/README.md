###### _by just-do-halee_

# _Today I learned_

## 2022-9-6

<br>

- Rust::ETC

  - [`digest crate`](https://crates.io/crates/digest)와 [`crypto-common crate`](https://crates.io/crates/crypto-common)는 Rust Standard Interface로서 표준 아키텍처(Trait)를 제공한다.

  - ***#[cfg(feature = "....")]*** 속성을 *struct* 위에, *enum* 위에, *fn* 또는 특정 *$expr*, *$stmt* 위에 넣든 모두 적용 가능하다.

  - ***#[no_mangle]*** 속성은 *struct* 위에, *enum* 위에, *fn* 위에, 변수 선언 위에 적용할 수 있으며, opt-level(컴파일 최적화 단계)에서 대상의 이름의 뭉개짐(변화)를 방지할 수 있다.

  - `상위 모듈(super mod)`이 `하위 모듈(mod)`의 private 변수 및 타입, 함수 등에 접근하는 것은 불가능하지만, *`하위 모듈에서 상위 모듈(super mod)의 private 대상은 접근 가능하며 불러올 수 있다.`*

<br>

- OS::Basic

  - ::Stack

    - **Rust**의 *Main Thread* 는 `8MiB`의 *Stack Memory*를 할당 받는다(기본값). 이후 *Child Thread*는 `2MiB`의 *Stack Memory*를 할당 받는다(기본값).

    - 중요한 점은 스택의 메모리는 바로 할당되는 것이 아니라는 점이다. High Address부터 시작점을 잡고, 필요한 만큼을 그때그때 사용해 Low Address 방향으로 늘려 나가는 것이다.

    - 만일 프로그램 스레드가 정해진(High Address로부터 시작되어 이미 할당되어 있는 특정 Low Address까지의) 공간의 Stack Memory를 넘어서서 이미 할당돼 있는 영역(또는 이미 정해진 지점)까지 침범한다면, 그 스레드는 운영체제(OS)에 의하여 강제종료(Terminated)될 것이다. 그리고 그것이 바로 ***Stack Overflow Error***이다.

  <br>

  - ::Heap

    - 한 프로세스 내의 모든 스레드는 공통의 *Heap Memory*를 공유한다.

    - *Heap Memory*는 Low Address로부터 스택의 바로 밑단 High Address까지 점차로 할당된다.

    - *Stack Memory*와는 정반대 방향으로 커진다.

    - Stack Memory와 Heap Memory의 서로 Address의 차이를 Bytes로 계산하면 겹칠 확률이 지극히 낮다. 그래도 만일 겹치게 되어 덮어쓰는 경우가 발생한다면 운영체제(OS)는 그것을 방지하고자 강압적으로 시행 종료할 것이다.

  <br>

  - ::ETC

    - 프로그램을 실행하여 프로세스를 생성할 때와 마찬가지로 `프로세스 내에서 스레드를 생성하는 것 또한 일정 딜레이가 소모`됨을 잊어서는 안 된다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
