###### _by just-do-halee_

# _Today I learned_

## 2022-9-30

<br>

- Rust::Test

  - `Doc tests`는 `private 필드에 접근할 수 없다.` 오직 `public한 API에만 문서를 작성하도록 하는 것이 바람직`하기 때문이다.

<br>

- Cryptography::Basic

  - `SHA-3`: 2012년 10월, NIST가 Keccak팀의 해시 함수를 새로운 SHA-3 해시 알고리즘으로 선정. 그러나 2015년 8월 NIST는 Keccak 팀의 기술 문서를 변경하여 새로운 표준안을 공개했다. 초기 Keccak 팀이 제출한 표준이 `Keccak`이고, 이후 NIST의 손길이 닿은 표준이 `SHA-3`이다. 둘은 다른 해시값을 계산하기에 구분해야 한다.

<br>

- Network::Basic

  - ::`DNS`(Domain Name System)

    - DNS 정점에 `ICANN`이라고 하는 비영리 단체가 군림한다. 이 비영리 단체는 전세계에 있는 `IP주소를 관리`하고 또 `Root name server들을 관리`한다.

      <br>

      - `Root name server`는 `a.root-servers.net`부터 `m.root-servers.net`까지 총 13대의 서버 컴퓨터를 총칭하는 말이며 이들은 전세계에 흩어져 `ICANN`에 의하여 관리되고 있다. 13대는 각기 또한 여러 대의 분산 컴퓨터로 처리되고 있어 최대 수백 대의 컴퓨터를 이루고 있다고 할 수 있겠다.

    <br>

    - ICANN 바로 밑에는 `Registry`(등록소)라고 하는 기관, 또는 기업들이 존재하며 `.com, .net .org...` 등 Top-level domain을 관리한다.

    - `Registrar`(등록대행자) 업체들은 실제 DNS를 등록하고 싶어하는 `Registrant`(등록자)들의 중간다리로서 등록을 대행해주는 사업을 한다.

    - ICANN의 `Root name server`에는 Top-level domain과 그것의 소유권자 `Registry`의 서버 도메인이 묶여서 데이터베이스에 저장되어 있다.

      ##### Example

      ```bash
      com NS a.gtld-servers.net
      ```

      - `.com` domain의 `Name Server`는 `a.gtld-servers.net`이다, 를 의미하며 이것이 Root name server에 기록되어 있다.

    <br>

    - `도메인 등록 과정`

      1. `본인의 Name server를 준비`해야 한다. ex:) a.iana-servers.net

      2. `Registrar(등록대행자)에게 본인이 원하는 도메인명, 본인의 Name server명을 전달`한다. ex:) my.com NS a.iana-servers.net

      3. `Registrar(등록대행자)는 Registry(등록소)에 2에서 전달 받은 그대로를 전달`해 요청한다.

      4. Registry(등록소)는 `등록소 자신의 Name server에 도메인명과 그 주인 쌍을 데이터베이스에 기록`한다(3에서 전달 받은 그대로). ex:) my.com NS a.iana-servers.net

      5. Registrant(등록자)인 `본인은 이제 요청했던 도메인에 원하는 IP를 묶어서 자기 자신의 Name server에 등록`하면 끝이 난다. ex:) 'my.com A 93.184.216.34' 를 본인의 Name server, a.iana-servers.net에 전송한다.

      <br>

      - ::참고::

        - "my.com NS a.iana-servers.net": `Record Type`이 `Name server Type`이라고 할 수 있다.

        - "my.com A 93.184.216.34": 이것은 `Address Type`이라고 할 수 있다.

    <br>

    - `도메인 연결 과정`

      1. 컴퓨터에 랜선이 꼽히는 등 인터넷이 연결되는 순간, ISP(Internet Service Provider)에 의해 기본 DNS Server 주소가 특정 주소로 자동으로 설정이 된다.

      2. 컴퓨터에 `my.com`을 쳐서 요청할 경우, 이 요청은 우선 ISP에 의해 기본 설정된 DNS Server에 전달될 것이다.

      3. ISP DNS Server는 `my.com`의 IP주소를 Root name server에게 직접 요청해 전달 받기를 기다릴 것이다.

      4. Root name server는 'com NS a.gtld-servers.net' 레코드에 의해 ISP DNS Server에게 `a.gtld-servers.net`이리로 가볼 것을 권할 것이다.

      5. 마찬가지로 Top-level domain server인 a.gtld-servers.net은 ISP DNS Server에게 'my.com NS a.iana-servers.net' 레코드를 확인하고, `a.iana-servers.net`에 가볼 것을 권할 것이고

      6. a.iana-servers.net는 자신의 'my.com A 93.184.216.34' 레코드를 확인하여 최종적으로 ISP DNS Server에게 `93.184.216.34` IP주소를 전달해

      7. ISP DNS Server는 최초 요청 컴퓨터에게 IP주소를 전달할 것이다.

      <br>

      - ::참고::

        - 모든 Name server는 기본적으로 Root name server의 주소를 전부 알고 있다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
