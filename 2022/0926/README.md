###### _by just-do-halee_

# _Today I learned_

## 2022-9-26

<br>

- Rust::SmartPointer

  - **_`Interior Mutability`_**

    - 내부에서라면 **_mut_** 키워드 없이도 값을 변경할 수 있도록 할 수 있는데, 그 규칙의 구현이다(Safe). 대표적으로 `Cell`과 `RefCell`이 있다.

  <br>

  - `Box` 는 값을 힙에 할당하되 drop될 시 자동 할당해제하는 역할을 한다. 그러나 이는 **_오직 단 하나의 Ownership_** 만을 허용한다.

  - `Rc` 는 Box와 마찬가지이되 **_여러 개의 Ownership_** 을 허용한다. 이 말은 **_무제한 Reference(&) SmartPointer 버전_** 임을 뜻한다. Reference 소유자가 모두 drop될 시 그제야 Heap에서 할당해제된다.

  - `Arc` 는 **_Rc 의 Thread Safe 버전_** 이다.

  - `Cell` 은 **_Interior Mutability_** 의 구현체이다.

    ```rust
    use std::cell::Cell;

    struct Person {
        biological_gender: bool,
        birth_day: u32,
        salary: Cell<u64>,
    }

    fn main() {
        let me = Person {
            biological_gender: true,
            birth_day: 19920125,
            salary: Cell::new(950000),
        }; // mut 키워드를 사용하지 않았지만,

        me.salary.set(80000000); // 이런 식으로 값을 변경할 수 있다.
        // * 주의할 점은 .set(..)에 넣을 값들은 모두 `Copy` 되어 Cell 내로 들어간다.
        // 또한 마찬가지로 .get()으로 꺼낼 시에도 `Copy` 되어 Cell 밖으로 빠져 나온다.
    }
    ```

  - `RefCell` 또한 Cell과 마찬가지로 **_Interior Mutability_** 의 구현체이다. 다만 **_Copy_** 를 이용하지 않고 **_Reference_** 를 이용하여 값을 변경한다.

    ```rust
    use std::cell::Cell;

    struct Person {
        biological_gender: bool,
        birth_day: u32,
        salary: RefCell<u64>,
    }

    fn main() {
        let me = Person {
            biological_gender: true,
            birth_day: 19920125,
            salary: RefCell::new(950000),
        }; // mut 키워드를 사용하지 않았지만,

        me.salary.replace(80000000); // 이런 식으로 값을 변경할 수 있다.

        // 또는 이런 식으로 값을 수정할 수도 있다.
        let borrow_mut = me.salary.borrow_mut();
        *borrow_mut = 80000000;

        // RefCell의 `.borrow()`, `.borrow_mut()` 또한
        // 일반 &, &mut 과 마찬가지로 `많은 &` vs `단 하나의 &mut`의
        // 규칙을 따라야 한다.

        // * 주의할 점은 RefCell의 경우 Compile Time이 아닌
        // Runtime 때에 위의 규칙이 적용되어, 옳지 않을 경우 panic! 한다.

        let borrow_mut_second = me.salay.borrow_mut();
        // 위 코드에 의해 프로그램 실행 도중
        // thread 'main' panicked at 'already borrowed: BorrowMutError'...
        // 메시지를 띄운다.
    }
    ```

<br>

- Rust::Trait

  - [`LowerHex`](https://doc.rust-lang.org/std/fmt/trait.LowerHex.html) 또는 [`UpperHex`](https://doc.rust-lang.org/std/fmt/trait.UpperHex.html) trait이 구현되어 있다면 이런 식으로 Hex(16진수) 값으로 포맷팅할 수 있다.

    ```rust
    let x = 42; // 42 is '2a' in hex

    assert_eq!(format!("{x:x}"), "2a");
    assert_eq!(format!("{x:#x}"), "0x2a");
    assert_eq!(format!("{x:X}"), "2A");
    assert_eq!(format!("{x:#X}"), "0x2A");

    assert_eq!(format!("{:x}", -16), "fffffff0");
    assert_eq!(format!("{:X}", -16), "FFFFFFF0");
    ```

  <br>

- Rust::Iterator

  - [`iter::once(..)`]() 는 어떤 값이든 그 단 하나의 요소를 지닌 Iterator로 만들어주는 함수이다.

    ```rust
    pub fn once<T>(value: T) -> Once<T> {
        Once { inner: Some(value).into_iter() }
    }
    ```

    와 완전히 동일하다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
