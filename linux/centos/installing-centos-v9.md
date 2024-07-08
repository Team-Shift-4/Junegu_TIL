# Installing CentOS (V9)

ISO 다운로드 사이트 : [https://www.centos.org/download/](https://www.centos.org/download/)

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 본 프로젝트는 CentOS-7-x86\_64-DVD-2009.ISO를 기준으로 작성한 문서이다.

#### VirtualBox OS 설치 추가

1. 새로 만들기(N) 버튼을 눌려 가상 머신을 만든다.

<figure><img src="../../.gitbook/assets/image (7) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 새로 만들기 : 신규 VM을 설치하고 생성한다.
* 추가 : 기존 만들어진 VM 을 추가한다.

2. 신규 가상머신 설정을 한다.

<figure><img src="../../.gitbook/assets/image (8) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 이름 : 가상머신 이름
* 폴더 : 만들어 가상머신 파일을 저장할 공간
* ISO 이미지 : 설치할 리눅의 ISO 이미지 경로

3. 설치 계정 및 옵션 설정을 한다.

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 사용자 이름 및 암호 : 서버에 설정할 초기 계정과 비밀번호
* 게스트 확장 : 네트워크 및 드라이브 마운트를 위한 추가 설정 설치

4. 하드웨어를 설정 한다.

<figure><img src="../../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 테스트 및 기본 설정용으로 사용할 예정으로 메모리는 4096MB, Processors 는 2로 설정한다.

5. 하드디스 설정

<figure><img src="../../.gitbook/assets/image (11) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 하드디스크 용량 설정
* 지금 새 가상 하드 디스크 만들기 : 초기 기본 고정 하드 용량 / 설정한 만큼 이미지 파일이 해당 용량으로 잡힌다.
* 기존 가상 하드 디스크 파일 사용 : VDI에 별도로 설정한 디스크 공간을 쉐어한다.
* 기본 OS와 기본응용프로그램들만 설치 후 소스나 기타 파일들을 마운트 잡아 사용할 것이기에 \[Create a Virtual Hard Disk Now] 로 40GB 로 설정할 예정이다.

6. 최종 확인 밑 Finish

<figure><img src="../../.gitbook/assets/image (12) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 설정한 정보들 확인 후 이상 없으로 \[완료] 버튼 클릭한다.

7. 완료

<figure><img src="../../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

* 정상적으로 설치되면 VMimages 리스트에 추가되며 상태가 \[ 종료 ] 에서 잠시후 \[ 실행 중 ]으로 변경된다.

8. 네트워크 설정

<figure><img src="../../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

* 어댑터 1,2 -> 네트워크 어댑터 사용하기 -> NAT / 호스트 전용 어댑터 -> 무작위 모드 모두 허용

### CentOS(V9) 설치

1. 설치시작

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

* Install CentOS 9 : 이 옵션을 선택하면 CentOS 9을 설치합니다. 선택하면 시스템에 CentOS 9이 설치되고, 설치 프로세스가 시작된다.
* Test this media & install CentOS 9 : 이 옵션을 선택하면 부팅 가능한 미디어의 무결성을 테스트한 후 CentOS 9을 설치다. 미디어가 올바르게 작동하는지 확인하기 위해 미디어를 테스트한 후에 CentOS 9을 설치할 수 있습니다. 이 옵션을 선택하면 시스템이 부팅 가능한 미디어를 테스트하고, 테스트가 완료되면 CentOS 9 설치 프로세스가 시작된다.

2. 언어 설정

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

* 한국어를 선택 한다.
* 키보드 레이아웃은 한국어 (101/104키 호환)을 추가한 후 기본값으로 선택 한다.

3. 설치 설정

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

* 주황색 삼각형 아이콘이 뜨는것들만 설정을 해주면 설치 시작을 할 수 있다.

4. 운영체제를 설치할 경로를 선택한다.

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

* 디스크 파티션 설정 할수있다.
  * **디스크 추가(A):** 사용 가능한 특수 디스크 또는 네트워크 디스크 추가한다.
  * **자동 설정(A):** Rocky Linux 설치 프로그램에서 기본 저장소를 자동으로 설정한다.
* 디스크를 고른 후 완료를 누른다.

5. 사용자 설정

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

* CentOS는 설치를 하면서 사용자와 ROOT 비밀번호를 설정할 수 있다.

6. 네트워크 설정

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

* **이더넷(enp0s3)** : 관리용 네트워크로 대시보드와 API에 접속할 수 있다.
* **이더넷(enp0s8)** : 인터넷과 외부에서 접속할 수 있는 유동 IP의 통로 역할을 한다.
* CentOS 9 버전에서는 enp0s3과 enp0s8둘다 자동으로 켬 상태로 되어있다.

7. 소프트웨어 선택

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

8. 설치 시작

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

* 설치가 끝나면 재부팅을 해준다.

9. 설치 완료

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

* 사용자 로그인 후 메인 화면이 뜨면 설치 완료이다.
