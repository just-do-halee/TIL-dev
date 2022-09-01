###### _by just-do-halee_

# _Today I learned_

## 2022-8-30

<br>

- Rust::Error

  - 라이브러리를 제작할 때는 자체 [`Error enum`]을 만들어 그것만 export시킬 것.
  - 어플리케이션을 제작할 때는 [`anyhow`](https://crates.io/crates/anyhow) crate과 같은 *통합 Result*를 이용해 모든 에러를 평등하게 return시킬 것.
  - 요점은 코드의 `사용자`가 누구인지를 파악하여 사용자 위주 error handling을 할 것.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
