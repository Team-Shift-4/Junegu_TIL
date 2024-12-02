# Linux basic knowledge

## Root

* 루트 계정은 가장 강력한 사용자 계정으로, 흔히 슈퍼유저 계정(superuser account)이라고도 한다.
* 이 계정은 시스템의 모든 명령어, 파일, 자원에 대한 무제한 접근 권한을 가지고 있다.
* 주로 소프트웨어 설치 및 제거, 시스템 전체 설정 변경, 사용자 계정 관리, 하드웨어 및 네트워크 설정 구성 등 넓은 권한이 필요한 관리 작업에 사용된다.
* 루트 계정으로 직접 로그인하지 말고 `sudo`(superuser do)를 사용한다
* 참고 사항:
  * 루트 디렉토리는 `/`로, 최상위 디렉토리이다.
  * 루트 홈 디렉토리는 `/root이`다.

## Case sensitive & Don't use space

* 시스템 사용의 많은 부분에서 대소문자 구분이 적용됩니다. 이는 파일 이름, 명령어, 흔히 사용되는 프로그래밍 언어에도 적용된다.
* 공백이 포함된 명령어를 사용하기 어렵다.

## Command prompt

* 명령 프롬프트는 다양한 운영 체제에서 사용자와 운영 체제 간 상호 작용을 위해 텍스트 기반 인터페이스를 제공하는 창구이다. 사용자 입력을 통해 명령을 입력하고 텍스트 기반 피드백을 받는다.
* 명령 프롬프트 구성 요소:

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

* 사용자(user): 현재 로그인한 사용자
* 호스트(host): 사용 중인 시스템 이름
* 프롬프트 기호(prompt symbol): 명령 입력 준비 표시

## File system path

* 절대경로(Absolute path):
  * 루트 디렉토리부터 시작하는 파일 또는 디렉토리 경로
  * 파일 또는 디렉토리 위치 명확하게 지정
  * 예: `/home/user/documents/report.txt`
* 상대  경로(Relative path):&#x20;
  * '.'(dot) 현재 디렉토리
  * '..'(double dot) 상위 디렉토리
  * 현재 작업 디렉토리 기준 파일 또는 디렉토리 경로
    * 루트 디렉토리 사용 불필요
    * 예:
      * `./document.txt`: 현재 디렉토리의 document.txt 파일
      * `../reports/report.txt`: 상위 디렉토리의 reports 디렉토리 내 report.txt 파일
      * `/home/user`: 루트 디렉토리의 home/user 디렉토리
* Same Directory File: document.txt
* Subdirectory: reports/report.txt
* Parent Directory: ../filelnParentDir.txt



