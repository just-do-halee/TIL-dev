###### _by just-do-halee_

# _Today I learned_

## 2022-8-30

- Rust:
  - 라이브러리를 제작할 때는 자체 [`Error enum`]을 만들어 그것만 export시킬 것.
  - 어플리케이션을 제작할 때는 [`anyhow`] crate과 같은 통합 Result를 이용해 모든 에러를 평등하게 반환할 것.
  - 결국, 요점은 코드의 `사용자`가 누구인지를 판단하여 사용자 위주로 Error를 핸들링할 것.
