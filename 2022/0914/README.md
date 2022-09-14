###### _by just-do-halee_

# _Today I learned_

## 2022-9-14

<br>

- ETC

  - ETC::Mac

    - Mac 시스템 업데이트 후, 터미널에 명령어를 시행시킬 시마다

      > '......' 명령어는 명령어 라인 개발자 도구가 필요합니다. 도구를 지금 설치하겠습니까?

      창이 뜨고, `설치` 버튼을 누르고 긴 시간 후 설치를 완료했음에도
      지속적으로 명령어 재시행 시마다 저 창이 뜨고,
      터미널창에서 `sudo xcode-select --install` 시행 시

      > xcode-select: error: command line tools are already installed, use "Software Update" to install updates

      이 문구가 뜬다면,

      ```bash
      $ xcodebuild -runFirstLaunch
      ```

      이를 시행시켜주면 된다.

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
