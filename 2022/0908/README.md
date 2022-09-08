###### _by just-do-halee_

# _Today I learned_

## 2022-9-8

<br>

- Rust::Function

  - **_`fn(..) -> ..`_** 함수 포인터 타입은 이미 선언된 `일반 함수`를 대변하는 타입으로, 클로저는 이 파라미터의 인자로 받아들여질 수 없다. 주변 변수를 캡처하지 않는 `|| { .. }` 함수는 `일반 함수`로서 기능한다. 캡처하는 순간 클로저가 된다.

  - **_`fn(..) -> ..`_** 은 Fn, FnMut, FnOnce를 모두 구현하므로, 반면에 클로저 자리의 인자로서 또한 기능할 수 있다.

    ```rust
    fn not_closure() {}

    fn return_fn(f: fn()) -> fn() {
        f
    }

    fn call_closure<F: FnOnce()>(f: F) {
        f();
    }

    // 일반 함수인 not_closure를 call_closure가 받아들일 수 있다.
    call_closure(return_fn(not_closure));
    ```

<br>

- Rust::ETC

  - _`String`_ 과 _`&str`_ 에는 **_`Into<PathBuf>`_** 가 이미 구현되어 있다.

  - *String*과 *&str*와의 관계와 비슷한 타입들

    ```rust
    // ↓ 소유권(ownership)이 존재하는 동적 메모리 할당(alloc) 대상 타입
    String, PathBuf, OsString, CString
    // ↓ 포인터(Pointer) 역할을 하는 borrowed reference
    &str, &Path, &OsStr, &CStr

    ```

  - _`Vec<T>`_ 의 _drain(..)_ 메소드는 인자로 _RangeBounds\<usize>_ 타입을 받으며 그 범위만큼의 Vector 요소를 _pop()_ 할 수 있다. 말 그대로 원본 변수로부터 뽑아내 없앤 다음에 그것을 그대로 리턴값으로 받아낸다. \* 여러 용도로 요긴하게 사용할 수 있다. for .. in .. 에서 즉각 사용 가능.

  - for `mut` A in B { C.. } 할 경우 C 내에서 A를 mutable로서 사용함을 명시한다.

  - **_`cargo install`_** 의 경우 여러 경로로 특정 **_bin_**ary파일만 골라서 설치할 수 있다.

    ##### example)

    ```bash
    $ cargo install PACKAGE_NAME --example EXAMPLE_NAME
    ```

  - **_`cargo install PACKAGE_NAME`_** 이 완료된 이후에는 터미널창에서 바로 *PACKAGE_NAME*만 입력해도 실행이 가능하다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
