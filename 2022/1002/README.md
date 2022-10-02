###### _by just-do-halee_

# _Today I learned_

## 2022-10-2

<br>

- Network::Basic

  - ::`Router`

    - U+ 공유기의 밑면을 확인할 시 `192.168.*.1` 아이피와 `#...` 샾으로 시작하는 패스워드를 확인할 수 있을 것이다.

    - 위의 아이피주소로 브라우저에 입력하여 접속하면 라우터 설정을 조작할 수 있다.

    - `NAT`(Network Address Translation) 메뉴로 들어가면 `포트포워딩`란이 존재하고, 원하는 포트를 입력한 뒤 공유기를 재시작하면 `해당 포트를 오픈할 수 있다.`

    - [FreeDNS](http://freedns.afraid.org/) Subdomain 등록과 함께 사용하면 손쉽게 자신의 아이피 및 포트를 전세계에 공개시킬 수 있다.

      ##### example:)

      ```shell
      plank.crabdance.com:6626
      ```

  <br>

  - ::`Packet`

    - 패킷 헤더에 포함되어 있는 `TTL` 값은 `Time To Live`의 약자로서 해당 `패킷의 유효 시간이 얼마나 되는지`를 나타낸다.

    - TTL 값이 `3`인 패킷은 `최대 3개의 라우터까지만` 거쳐갈 수 있다.

    - TTL 값이 20인 패킷은 10개의 라우터들을 모두 거치고도 10번을 더 거칠 수 있는 것이다.

    - TTL의 존재 목적은 엉킨 라우팅으로 인해 해당 `패킷이 영원히 라우터 사이를 떠돌아다니는 일을 미연에 방지하고자 하기 위함`이다.

    - 참고로 이 TTL은 운영체제에 따라 값이 달라지기에 `ping` 명령어로 특정 어드레스로부터 TTL 값을 취득하여 기본적으론 해당 노드가 어떤 운영체제를 이용하고 있는지를 알아낼 수도 있다.

      ```shell
      linux = 64
      windows = 128
      cisco = 256
      ```

    - TTL 값은 `라우터를 거쳐갈 때마다 1씩 줄어들어 다음 라우터에 전달`된다. 이 점이 요점이다.

    - 고로 출발지로부터 전달 받은 패킷의 `TTL이 적으면 적을수록` 해당 출발지는 현재 `자신의 위치로부터 멀리 떨어져 있음`을 시사한다.

<br>

- CommandLineTools

  - [`curl`](https://curl.se): 서버에 데이터를 전송하는 툴이다. 대부분의 프로토콜을 지원한다(HTTP, FTP, POP3, ...).

    - 자주 사용하는 옵션

      - `-d`, --data: POST 요청 시 `보낼 데이터`를 입력한다.
      - `-H`, --header: 요청에 제공할 `헤더`를 설정한다.
      - `-X`, --request: 요청 `메소드`를 설정한다. 예:) `GET, POST`

    <br>

    `URL`형식

    ```shell
    curl -d "key1=value1&key2=value2" \
    -H "Content-Type: application/x-www-form-urlencoded" \
    -X POST http://localhost:8000/example
    ```

    `JSON`형식

    ```shell
    curl -d '{"key1":"value1", "key2":"value2"}' \
    -H "Content-Type: application/json" \
    -X POST http://localhost:8000/example
    ```

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
