###### _by just-do-halee_

# _Today I learned_

## 2022-10-1

<br>

- Network::P2P

  - ::`Gossip`

    - `Gossip Protocol`이란, 분산 환경에서 노드 간에 정보를 공유하는 프로토콜이다. 마치 바이러스가 퍼지는 방식과 유사하게 동작하기에 `전염병 프로토콜`(Epidemic Protocol)이라고도 할 수 있다.

    - `마스터가 없는` 대신 각 노드들이 주기적으로 `UDP/TCP IP`로 메타 정보를 주고 받아 결국 다수의 노드로부터 인증을 받은 정보가 `다수의 합의로 도출` 되는 데 프로토콜의 의의가 있다.

    - 분산 환경에서 동기화 알고리즘인 Paxos나 Raft의 복잡함 대신 `가볍고 빠른 방식`이라고 할 수 있다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
