###### _by just-do-halee_

# _Today I learned_

## 2022-9-19

<br>

- Network::Basic

  - ::Client_Server

    - `클라이언트-서버(Client-Server)` 아키텍쳐의 장점

      1. 처리량의 배분을 한쪽으로 크게 몰아서 기업과 개인간의 상호운용성을 획득할 수 있다.
      2. 개인의 입장인 클라이언트측에서 값비싼 컴퓨팅 연산을 기업의 거대 컴퓨터에 맡길 수 있고,
         그 시간 동안 클라이언트는 가벼운 테스크들을 도맡아서 **_병렬적_** 으로 처리할 수 있다.
      3. 클라이언트들은 값비싼 연산을 위한 수많은 종류의 **_의존성들로부터 탈피_** 하는 것이 가능하다.

  - ::OSI

    - `OSI 모형(Open Systems Interconnection model)`이란 국제표준화기구(ISO)에서 개발한 모델로, 컴퓨터 네트워크 프로토콜 통신을 계층화하여 설명한 것이다. 대체로 `OSI 7계층`이라고 불린다.

    <br>

    - 왜 통신에는 규약이 필요할까?

      - 만일 두 생명종이 존재한다고 치자. 그 두 종 간에 서로 원활한 소통이 이루어지기 위해서는 두 종이 각기 쓰는 언어의 통일성이 존재해야 한다. 그래야지만 서로 원하는 것을 표현할 수 있고 또 알아들을 수 있는 것이다.
      - 언어를 알아들을 수 없다면, 상대방의 요청에 어떻게 응답해야 할지 확실하지가 않다. 이를 프로그래밍화한다는 것은 가히 불가능하다.

      - 또한, 언어를 어떻게든 해독하여 알아들 수 있다고 치자. 그런데 그것은 단 하나의 타 생명종에 대해서 이루어진 것일 뿐이다. 만일 수십 생명종이 더 존재한다고 치고 그들 모두가 각자 다른 언어를 사용한다고 친다면, 이 종과 저 종 간에는 이런 언어를 수고해서 번역해서 써야 하고, 또 이 종과 저 종 간에는 또 다른 번역을 이용해서 써야 하고... 재앙이 벌어질 것이다.
      - 언어를 단 하나로 통일한다는 것은 상상 이상의 효율성을 가져다 준다.

      <br>

      > `![생각]!` 현실 세계에서의 언어

      > 이를 가능케 하기 위해서는 최소한 두 언어 사이의 `패턴화`가 동등하게 이루어져야 한다.
      > 패턴화가 동등해질 수 있다고 한다면 최소한의 시간과 노력으로도 서로의 언어를 `통역`할 수 있다.
      > 한마디로 쉽게 이 언어 저 언어 배울 수 있는 것이다.
      > 어순이 동일한 한국어와 일본어를 보라.
      > 더 나아가 문법이 동등한 수많은 프로그래밍 언어들을 보라.
      > 단 한 가지만 유창하게 할 수 있어도 다른 언어 또한 비교적 금방 배울 수 있다.
      > 물론 앞서 말했다시피 번역해야 할 수고 자체를 덜 수 있다면 최고이고, 그것은 모두가 처음부터 단 하나의 언어를 사용해 익히면 되는 것이다.

      <br>

    - 규약을 보다 세분화하여 계층화시켜두면, 각 계층(가장 기초적인 패킷 전송부터 가장 상위의 바이트 해석 방식까지) 각기 병렬적으로 Innovation이 이루어짐에 있어서 방해를 받지 않고 보다 효율적으로 관리 및 개선이 이루어질 수 있다.

    - OSI Model

      ```markdown
      Layer 7 - Application - HTTP/FTP/gRPC

      # 어플리케이션 개발자가 읽고 쓰는 최상위 단계.

      Layer 6 - Presentation - Encoding, Serialization

      # 위의 HTTP, gRPC 등 구성이 어찌됐든 문자열(String) 자체를 전송하는 것이기에, 이를 어떤 형태로 전송하고 수신할지를 결정하는 단계이다. 가장 대표적으로 UTF-8.

      Layer 5 - Session - Connection establishment, TLS

      # 연결의 시작 및 종료, 도중 오고가는 데이터들의 동기화 등 전체적인 데이터 송수신 흐름의 보존과 통제를 담당하고 있다.

      Layer 4 - Transport - UDP/TCP

      # 데이터 자체를 어떤 방식으로 전송할 지를 정하는 단계이다. 포트(Port)를 사용할지 말지도 정할 수 있다. `Segment` 또는 `Datagram`을 송수신한다.

      Layer 3 - Network - IP

      # 도착지를 결정한다. 라우팅(Routing)도 포함된다. `Packet`을 송수신한다.

      Layer 2 - Data link - Frames, Mac address Ethernet

      # 하드웨어 고유 주소에 `Frame`을 송수신하는 단계이다.

      Layer 1 - Physical - Electric signals, fiber or radio waves

      # 물리적 매게체를 통해 신호를 전달하는 단계이다.
      ```

    <br>

    - Example (Sender)

      ```markdown
      Layer 7 - Application

      # HTTPS 서버로 JSON 형식의 데이터를 POST 요청해 보낸다.

      Layer 6 - Presentation

      # JSON 형식 데이터를 Bytes 스트링으로 직렬화한다.

      Layer 5 - Session

      # TCP/TLS 연결을 성사시킨다.

      Layer 4 - Transport

      # SYN 요청을 타겟 포트 443에 보낸다.

      Layer 3 - Network

      # SYN을 IP 패킷에 포함시키고 목적지 IP를 덧붙인다.

      Layer 2 - Data link

      # 각 패킷들은 하나의 `Frame` 안으로 들어가며 목적 MAC 주소가 덧붙인다.

      Layer 1 - Physical

      # 각 `Frame`은 연속된 비트들로 전환되며 그것은 다양한 방식으로 전기적 신호로 변환돼 전송된다.
      ```

    - Example (Receiver)

      ```markdown
      Layer 1 - Physical

      # 수신된 전기적 신호들은 디지털 비트로 전환된다.

      Layer 2 - Data link

      # Layer 1으로부터 수신된 비트들은 `Frame`으로 구성돼 조립된다.

      Layer 3 - Network

      # Layer 2로부터 구성된 `Frame`이 IP 패킷으로서 완성된다.

      Layer 4 - Transport

      # Layer 3로부터 완료된 IP 패킷들은 TCP segments로 다시 이루어진다.

      # 만일 Segment가 SYN이라면, 더 이상 이 위의 레이어로 진행하지 않아도 된다.

      Layer 5 - Session

      # 연결 세션이 성사되고, 식별된다.

      # 오직 필요할 때에만 이 레이어에 도달한다.

      Layer 6 - Presentation

      # Bytes 문자열을 App을 위한 JSON 형태로 역직렬화한다.

      Layer 7 - Application

      # App이 그 JSON을 POST 요청으로 이해하고, 함수를 호출한다.
      ```

    <br>

    - ::참고::

      - OSI model은 독립적으로 개선될 수 있으나, 특정 계층이 정상적으로 작동되기 위해서는 그 하위 모든 계층이 정상적으로 작동할 수 있어야 한다.

      - **_`MAC`주소(Media Access Control Address)_**: 통신을 위한 네트워크 인터페이스에 할당된 고유 식별자이다. 이더넷과 와이파이, 대부분의 [IEEE 802](https://ko.wikipedia.org/wiki/IEEE_802) 네트워크 기술에 네트워크 주소로 사용된다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
