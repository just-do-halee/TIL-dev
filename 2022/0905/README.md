###### _by just-do-halee_

# _Today I learned_

## 2022-9-5

<br>

- Rust::Thread

  - ::Channel

    - Sender\<T>, Receiver\<T> Struct

      ```rust
      // in standard library
      use std::sync::mpsc::{ Receiver, Sender };
      ```
      or
      ```rust
      // in crossbeam
      use crossbeam::channel::{ Receiver, Sender };
      ```

    <br>

    - **_`Receiver<T>`_** 는 *iter()*, *try_iter()*, *into_iter()* 가 구현되어 있다(**IntoIterator** Trait).

      ```rust
        fn recv_and_print(rx: Receiver<u32>) {
          for number in rx {
            println!("{number}");
          }
        }
      ```

    - 고로 이런 식으로 for .. in .. 반복문도 구현할 수 있다.

    - *rx.iter().next()* 는 채널 큐(Channel Queue)에 무언가가 들어올 때까지 해당 스레드를 Blocking하므로 위의 예제에서 number가 온전히 존재할 때까지 스레드는 코드 블록킹 상태가 된다.

    - *rx.recv()* 또한 마찬가지다. 다만 *rx.iter().next()* 는 ***채널 큐가 완전히 비어 있으며 채널간 완전히 연결이 끊겼을 때*** *None*을 리턴하는 반면, *rx.recv()* 는 그 경우 *RecvError*를 리턴한다. for .. in .. 일 경우 None이 리턴될 경우 자동으로 반복문으로부터 벗어난다.

    <br>

    - **_`Sender<T>`_** 는 *bounded channel*일 경우, 만일 채널 큐(Channel Queue)가 가득 차 있을 경우, 해당 스레드는 [Blocking] 상태로 전환한다.

       ```rust
        let (tx, _) = channel::bounded::<u32>(6);
        tx.send(42).unwrap();
        // 채널 큐가 이미 6의 길이만큼 가득 차 있다면 한 칸이라도 비어지기 전까지는 위 코드는 완수되지 못하고 다음 println!은 시행되지 못한다.
        println!("complete.");
       ```
    
    - 만일 *unbounded channel*일 경우 Blocking되지 않는다.
    
    - *tx.send(..)* 는 *Result<(), SendError\<T>>* 를 리턴한다. 채널 큐가 비어 있고 또 연결이 끊겨 있을 경우 *SendError\<T>* 가 리턴된다.

    <br>

    - ::참고::

      - 송신측이든 수신측이든 어느 한쪽의 `모든 복사된` 채널들이 `drop`될 경우 채널의 연결은 끊긴 것으로 간주된다.

      - 채널 큐(Channel Queue)가 비어 있다는 말은 말 그대로 큐가 비어 있는 것이다.

      - ***`tx`***: 송신측(transmitting side).

      - ***`rx`***: 수신측(receiving side).


<br><br>

##### **_[`| Back to list |`](../../README.md)_**
