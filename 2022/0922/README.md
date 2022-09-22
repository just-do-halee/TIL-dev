###### _by just-do-halee_

# _Today I learned_

## 2022-9-22

<br>

- Rust::Basic

  - `Vec<T>` 타입은 `Heap`에 `capacity만큼 할당`되어 있다가, 만약 그 용량을 초과할 경우(그럴 경우) `이전의 capacity * 2` 만큼의 새로운 용량의 메모리를 `Heap의 다른 곳에 재할당`한 뒤, `이전까지 보관하고 있던 내용들을 새로 할당한 메모리에 복사-붙여넣기` 한다.

  - 현대의 `CPU`는 `근접한 메모리(contiguous memory)를 선호`하므로, 또한 `예측하기 쉬운 패턴을 선호`하므로, 마지막으로 Vec\<T> 타입을 array `slice 타입으로 변환하기 용이`하게 하기 위함으로 Rust Vec 타입은 이러한 방식을 채택하고 있다. 선호한다는 말은 그만큼 여러 이점들이 존재하며, 그에 부합하는 최적화가 존재한다는 말이다.

  - `Associated functions`란, 타입 자체에 연결된, OOP에서의 Static functions와 맥락을 같이 하는 함수를 칭한다.

    ```rust
    struct Foo;
    impl Foo {
        /// self parameter가 제외되어 있다.
        /// 즉, 타입 자체로부터 호출할 수 있는 함수이다.
        /// ex:) Foo::associated_function();
        fn associated_function() {
            println!("This is a function associated with the struct.");
        }
    }
    ```

<br>

- Rust::SmartPointer

  - `String` 타입은

    ```rust
    pub struct String {
        vec: Vec<u8>,
    }
    ```

    으로 구성되어 있고,

  - `Vec` 타입은

    ```rust
    pub struct Vec<T, A: Allocator = Global> {
        buf: RawVec<T, A>,
        len: usize, // * length
    }
    ```

    이렇게 구성되어 있으며,

  - `RawVec` 타입은

    ```rust
    pub(crate) struct RawVec<T, A: Allocator = Global> {
      ptr: Unique<T>, // * pointer: *const T
      cap: usize, // * capacity
      alloc: A,
    }
    ```

    이렇게 구성되어 있으므로 다시 결국 `String` 타입은,

    ```rust
    1. length (= `len`) // 문자열의 길이
    2. capacity (= `cap`) // 실제 Heap에 할당되어 있는 총 용량
    3. pointer (= `ptr`) // 문자열 첫 번째 바이트 문자의 주소
    ```

    이렇게 메타데이터를 지니는 `Smart Pointer`겠다.

    _drop() 될 경우 할당해놓은 모든 것이 할당해제된다._

    <br>

    - ::참고::

      - String slice(`&str`)도 그렇고, 일반 Slice(`&[..]`)도 그렇고,

        ```rust
        1. length (= `len`) // 길이
        2. pointer (= `ptr`) // 첫 번째 요소 주소값
        ```

      `이 두 가지만으로` 기존 Heap에 할당돼 있던 String 및 Vec를 요소를

      가리키는 게 가능하므로 `값싼 Reference`로서 기동할 수 있다.

<br>

- Rust::ETC

  - `String` 및 `&str` 타입은 오직 유효한 UTF-8 문자열만 취급해 담을 수 있다.

  - `char` 타입은 *`4` bytes*로서 마찬가지로 오직 유효한 유니코드 형태만이 담긴다.

  - `enum` 타입의 크기(`Size`)는 `가능한 내부 값 중 가장 큰 크기`를 지닌 타입 하나만큼의 크기로 결정되어진다.

  - `enum` 타입 내에 하나라도 단순 열거형이 아닌 모종의 값을 지니는 _struct가 포함되어 있다면, Enumerable digit으로 표현될 수 없다._

  - `enum` 타입이 `Default Trait`을 구현하고 있다면,

    ```rust
    #[derive(Default)]
    enum Foo {
        Bar(String),
        Baz,
    }
    fn main() {
        let default_foo = Foo::Bar;
        assert_eq(default_foo, Foo::Bar(String::new()));
    }
    ```

    이런 식으로 사용될 수 있다.

<br>

- ETC

  - 리눅스 또는 x86 Mac에서 사용되는 디버깅 툴인 **_`gdb`_** 는 M1(arm64) 이상 Mac에서는 실행되지 않으므로 대신하여 [**_`lldb`_**](https://lldb.llvm.org/use/map.html) 를 사용한다.

    ```bash
    $ lldb ./실행파일
    ```

    실행 이후,

    ```bash
    $ list
    ```

    명령어를 통해 전체 소스를 확인하고,

    ```bash
    $ b main.rs:12
    ```

    위와 같은 예시로 중단점(breaking point)를 원하는 만큼 잡은 뒤,

    ```bash
    $ r
    ```

    을 쳐서 실행시킨다.

    만일 현재 스코프 내의 Local Variable 및 Argument들을 확인하고 싶다면,

    ```bash
    $ fr v # 혹은 frame variable
    ```

    로 확인하고 특정 Variable을 다른 표현식으로 포맷팅하여 확인하고 싶다면,

    ```bash
    $ fr v -f x foo # x는 hex이고, d이면 digit이다.
    # 혹은 frame variable --format x bar
    ```

    로 확인할 수 있다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
