###### _by just-do-halee_

# _Today I learned_

## 2022-10-4

<br>

- Blockchain::Bitcoin

  - 비트코인 마이너들에게 `블록 전파 속도`는 그들의 마진 그 자체이기에 중요 포인트 중 하나이다. 2016년 코어 팀 일원인 Matt Corallo에 의해 `UDP 기반 블록 전파 엔진 FIBRE`가 개발되었고 이는 높은 수준의 퍼포먼스를 보여 주었다.

  - 새 노드가 알려진 피어와 TCP 연결이 성사되면(보통 8333 포트이다) 새 노드는 `Handshake`를 시도한다.

  - Handshake 시 전송되는 메시지는 이와 같다.

    ##### [`참고`](https://en.bitcoin.it/wiki/Protocol_documentation#version)

    ```toml
    # * version serializing *
    # ------------------------
    nVersion # P2P 프로토콜 버전. (예를 들면, 70002)
    nLocalServices # 내가 제공하는 서비스 리스트
    nTime # 현재 시간
    addrYou # 상대 노드 주소
    addrMe # 자기 노드 주소
    subver # 내 서브 버전. (예를 들면, /Satoshi:0.9.2.1/)
    BestHeight # 내 체인의 길이
    # ------------------------
    ```

    ![`Handshake`](./img/handshake.png)

    요청이 수락되었다면 [`Verak`](https://en.bitcoin.it/wiki/Protocol_documentation#verack) 메시지를 답변으로 전송한다.

  - 새 노드가 첫 피어를 찾는 방법 중 대표적으로 DNS seeds를 사용하는 것이 있다.

    - 소스코드에 하드코딩된 [`도메인 주소 리스트`](https://github.com/bitcoin/bitcoin/blob/master/src/chainparams.cpp#L121) 중 하나의 도메인(seed)에 접근하면 IP주소 리스트를 응답해주는데, 그것들은 어느 정도 인증된 노드의 아이피 주소들이니 이를 통하여 최초로 Handshake하면 되겠다.

      ##### 직접 살펴보자.

      ```shell
      $ nslookup dnsseed.bitcoin.dashjr.org
      ```

  - Handshake가 성공적으로 완료되면, 새로 들어온 노드는 자신의 IP 주소가 담긴 `addr` 메시지를 상대 노드에게 보낼 것이다. 메시지를 받은 상대 노드는 그 자신의 연결된 이웃 노드들에게 새 노드의 메시지를 그대로 전파할 것이고 이웃 노드들은 신입 노드를 인식할 것이다(이후에 더 잘 연결될 것이다). 그리고 새로 들어온 노드는 `getaddr` 메시지를 연결된 상대 노드에게 한 번 더 보낼 것이고, 상대 노드는 자신의 이웃 노드들의 아이피 주소가 담긴 `addr` 메시지를 여러 번에 걸쳐 전송할 것이다(리스트).

    ![`Getaddr`](./img/getaddr.png)

  - 한 노드는 반드시 다양한 경로의 다양한 최소 몇몇의 노드들과 연결되어야 한다. 기존 노드들은 언제나 종료되거나 아니면 다시 새로 들어오거나 유기적이기에 반드시 개개의 노드들은 새로운 노드들을 발견할 필요성이 있다.

  - 가장 최근에 Handshake에 성공해 들어온(Bootstrapping) 노드는 잠시 노드를 재실행하였을 시 다시 발빠르게 방금 전까지 연결해왔던 노드들과 재연결이 될 것이나, 그렇지 못한 노드들은 다시 하드코딩된, 혹은 다른 경로로 알려진 seed 노드에 요청해 새 진입이 가능할 것이다.

  - 한 번 이상 연결했던 피어들은 이 피어 리스트에 기록이 된다.

    ```json
    [
      {
        "addr": "85.213.199.39:8333",
        "services": "00000001",
        "lastsend": 1405634126,
        "lastrecv": 1405634127,
        "bytessent": 23487651,
        "bytesrecv": 138679099,
        "conntime": 1405021768,
        "pingtime": 0.0,
        "version": 70002,
        "subver": "/Satoshi:0.9.2.1/",
        "inbound": false,
        "startingheight": 310131,
        "banscore": 0,
        "syncnode": true
      },
      {
        "addr": "58.23.244.20:8333",
        "services": "00000001",
        "lastsend": 1405634127,
        "lastrecv": 1405634124,
        "bytessent": 4460918,
        "bytesrecv": 8903575,
        "conntime": 1405559628,
        "pingtime": 0.0,
        "version": 70001,
        "subver": "/Satoshi:0.8.6/",
        "inbound": false,
        "startingheight": 311074,
        "banscore": 0,
        "syncnode": false
      }
    ]
    ```

  - 노드들은 서로간에 주기적으로 `ping/pong` 메시지를 보내 현재 온라인 상태인지 확인할 수 있다. 이때 ping에 무작위 숫자 하나(`nonce`)를 붙여 보낸 뒤 되돌아온 pong에도 자신이 보낸 무작위 수가 덧붙여 있는지 확인하여 옳게 된 `ping/pong`인지 확인할 수 있다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
