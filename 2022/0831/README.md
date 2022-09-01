###### _by just-do-halee_

# _Today I learned_

## 2022-8-31

<br>

- Rust::Error

  - **_`#[non_exhaustive]`_** 속성은 [`Error enum`] 위에 항상 붙여줘야 한다.

    - 붙여줄 경우 그 enum을 외부 모듈에서 _`match`_ 해 사용할 시

    ```rust
      ...
      use FooError::*; // enum FooError { FirstError, SecondError }
      match e {
        FirstError => ...,
        SecondError => ...,
        _ => ...,
      }
      ...
    ```

    - 반드시 더 이상 매칭할 분류가 존재하지 않음에도 `그 외의 모든 것`을 뜻하는 _\_(언더스코어)_ 매칭을 사용해야만 하고, 이는 사용자 측면에서 라이브러리가 업데이트 됐음에 사용자 코드를 깨지 않아도 되게끔 하는, Error 측의 의미론적 major버전의 붕괴를 방지할 수 있게 한다.

  <br>

- Rust::Test

  - **_`#[should_panic]`_** 속성은 [`Test fn`] 위에 붙여 줄 수 있으며, 이는 test running 시 이 함수가 _panic!()_ 되어야 함을 선언한다.

  - `cargo test` 시 **_Doc-tests_** 는 오직 --lib crate에서만 동작하며 --bin crate에서는 동작하지 않는다.

  - 최대한 모든 테스트는 라이브러리 내에서 이루어지도록 하고, 어플리케이션에서는 최소한의 코드로 작동하게끔 해야 한다.
    - 그럼에도 불구하고 꼭 어플리케이션 테스트를 하고 싶다면, 별도로 테스트용 바이너리를 만들어 _cfg!(target_os = "...")_ 를 이용 각 OS에 맞춘 _process::Command::new("...")..._ 커맨드 입력으로 직접 바이너리를 실행시키도록 해 테스트를 만들어 본다.

  <br>

- Rust::Bench

  - Benchmarking 시에는 정말 퍼포먼스가 중요한 지점만을 골라 파악하며, 최대한 실제 제품 시의 환경(Hardware, OS, Rust Version, etc...)과 똑같은 환경을 구축해 시행해야 한다.
  - [`criterion`](https://crates.io/crates/criterion)과 같은 외부 benchmarking core crate를 사용할 시에는 항상 Cargo.toml 내에

    ```toml
    [[bench]]
    name = "rs_file_name_inside_benches"
    harness = false
    ```

    - _harness_ 를 `false`로 해야만 한다. _harness_ 는 (built-in test도 포함) (현) nightly버전의 unstable한 feature들을 이용한 Rust built-in benchmarking을 활성화시키는 옵션이다. 기본값은 true로서 켜져 있다.

  <br>

- Rust::Log

  - Standard library에 `log`는 포함되지 않는다. 그러나 Official Library는 존재하는데, 그것이 [`log`](https://crates.io/crates/log) crate이다.

  ```rust
    use log::{error, warn, info, debug, trace};
    // 런타임 때 log-level이 결정되는데,
    // log-level이
    error!("Serious stuff"); // error일 경우에는 error만,
    warn!("Pay attention"); // warn일 경우에는 error, warn까지만,
    info!("Useful info"); // info일 경우에는 error, warn, info,
    debug!("Extra info"); // debug일 경우에는 error, ..., debug,
    trace!("All the things"); // trace일 경우에는 trace, 위의 모든 것 포함해
    // 로그가 출력이 된다.
    // * 참고
    // error = highest priority
    // trace = lowest priority
  ```

  - [`log`](https://crates.io/crates/log) crate은 단순히 모든 [`logger library`](https://docs.rs/log/latest/log/#available-logging-implementations)들이 따라야 하는 밑바탕 trait을 구현한 추상 인터페이스의 뼈대일 뿐으로, 실제 logging이 시행되기 위해서는 [`상위 라이브러리`](https://docs.rs/log/latest/log/#available-logging-implementations)를 또 추가해 execute해야 한다.

  <br>

- Rust::ETC

  - 1.59 버전 이후부터는

  ```rust
    let _ = foo(); // 를
  ```

  ```rust
    _ = foo(); // 이렇게 바꿔 사용한다.
  ```

  <br>

- OS::Basic

  - 컴퓨터가 켜지면, 운영체제(Operating System: OS)의 프로그램 코드는 RAM 상에 올라간다. 이때 그 올라간 공간을 `OS space`라고 부른다. 반면, 남은 공간을 `User space`라고 부른다.
  - 프로그램이라는 것은 기본적으로 사람이 읽을 수 있는 `Source Code`로서 존재하다가, Compiler라는 다른 프로그램을 사용하여 컴퓨터가 읽어낼 수 있는 `Machine Code`로 변형시킨 그, 0과 1만으로 이루어진 `바이너리 파일`(Binary File: Bin File)자체를 의미한다.
  - 바이너리 파일은 `Hard Disk`에 존재하다가, 실행 될 시에 `RAM`의 User space로 옮겨지는데, `CPU`가 그 Machine Code를 하나씩 하나씩(Instruction by Instruction) 읽어들여 계산해(Calculate) 프로그램을 진전(실행)시킨다.
  - 이후 종료된 프로그램은 RAM 상에서 (공간이 무제한이 아니기에, 다른 프로그램도 와야 하므로) 지워져야 하고, 이를 `메모리상에서 Free`되었다고 표현한다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
