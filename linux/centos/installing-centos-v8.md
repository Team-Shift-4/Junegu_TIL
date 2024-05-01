# Installing CentOS (V8)

ISO 다운로드 사이트 : [https://www.centos.org/download/](https://www.centos.org/download/)

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

* 본 프로젝트는 CentOS-Stream-8-x86\_64-latest-dvd1.ISO를 기준으로 작성한 문서이다.

#### VirtualBox OS 설치 추가

1. 새로 만들기(N) 버튼을 눌려 가상 머신을 만든다.

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

* 새로 만들기 : 신규 VM을 설치하고 생성한다.
* 추가 : 기존 만들어진 VM 을 추가한다.

2. 신규 가상머신 설정을 한다.

<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

* 이름 : 가상머신 이름
* 폴더 : 만들어 가상머신 파일을 저장할 공간
* ISO 이미지 : 설치할 리눅의 ISO 이미지 경로

3. 설치 계정 및 옵션 설정을 한다.

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

* 사용자 이름 및 암호 : 서버에 설정할 초기 계정과 비밀번호
* 게스트 확장 : 네트워크 및 드라이브 마운트를 위한 추가 설정 설치

4. 하드웨어를 설정 한다.

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

* 테스트 및 기본 설정용으로 사용할 예정으로 메모리는 4096MB, Processors 는 2로 설정한다.

5. 하드디스 설정

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

* 하드디스크 용량 설정
* 지금 새 가상 하드 디스크 만들기 : 초기 기본 고정 하드 용량 / 설정한 만큼 이미지 파일이 해당 용량으로 잡힌다.
* 기존 가상 하드 디스크 파일 사용 : VDI에 별도로 설정한 디스크 공간을 쉐어한다.
* 기본 OS와 기본응용프로그램들만 설치 후 소스나 기타 파일들을 마운트 잡아 사용할 것이기에 \[Create a Virtual Hard Disk Now] 로 40GB 로 설정할 예정이다.

6. 최종 확인 밑 Finish

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

* 설정한 정보들 확인 후 이상 없으로 \[완료] 버튼 클릭한다.

7. 완료

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

* 정상적으로 설치되면 VMimages 리스트에 추가되며 상태가 \[ 종료 ] 에서 잠시후 \[ 실행 중 ]으로 변경된다.

8. 네트워크 설정

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* 어댑터 1,2 -> 네트워크 어댑터 사용하기 -> NAT / 호스트 전용 어댑터 -> 무작위 모드 모두 허용

### CentOS(V8) 설치

