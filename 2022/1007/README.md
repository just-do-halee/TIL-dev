###### _by just-do-halee_

# _Today I learned_

## 2022-10-7

<br>

- OS::Thread

  - `Hyper-Threading`: 인텔이 최초로 구현한, CPU 한 개에 두 개의 가상 실행 코어를 장착해 마치 두 개의 코어가 있는 것처럼 시행될 수 있는 기술이다.

  - `Tokio` Runtime과 같은 `Rust async runtime` 내에서 이 CPU코어 개수에 맞는(위의 하이퍼스레딩 기법이 적용된 CPU라면 본래 물리적 CPU * 2개의 개수만큼) 스레드풀(ThreadPool)을 만들어 놓고 Task를 넘겨 각 놀고 있는 Worker(OSThread)들에게 할당해 실행시키는데 이를 `비동기`라고 한다.

  - 위와 같은 구현을 추상화한 것이 `Green Thread`이다.

  <br>

    - ::참고::

      - 논리적 CPU개수를 초과한 ThreadPool의 사이즈를 Spawn할 경우 그 일부는 부족한 CPU에 끼워넣기로 시행되어야 하므로 지속적인 Context Switching Overhead에 시달리게 될 것이다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
