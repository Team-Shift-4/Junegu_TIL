# CAPSTON DESIGN

## 캡스톤 디자인(회의 프로그램)

### 프로젝트 소개

브레인 스토밍, 마인드맵, AI를 이용한 요약, 실시간 채팅 등을 활용하여 도움을    주는 회의 프로그램



### 주요 서비스

* 메인 페이지 디자인
* 화상회의
* 브레인스토밍
  * 마인드맵
  * SWOT
  * 여섯가지 사고모자
  * 크레이지 에이트
* 팀 서비스
* 회의방 서비스
* 배포



#### 메인 페이지 디자인

* 메인화면에 스크롤스파이 기법으로 만들어진 Navbar를 이용해 서비스에 대한 소개를 보여준다.
* 해당 서비스에 대한 자세한 설명을 보고 싶을 때 필요한 상세 페이지가 각 항목마다 구현되어 있다.
* AWS에 S3를 사용하여 프로필 이미지 관리

{% embed url="https://www.figma.com/design/wcckIrZ2fW8xMyS2T117em/%EC%BA%A1%EC%8A%A4%ED%86%A4?node-id=0-1&t=aTRc3aLDAAyizyKv-0" %}



#### 화상 회의

* 실시간 화상회의 및 채팅 기능 제공
  * 사용자들은 실시간으로 영상 및 음성을 통해 회의에 참여할 수 있으며, 텍스트 채팅 기능을 통해 메시지를 주고받을 수 있다.
* 화면 공유 기능 제공
  * 회의 참가자는 자신의 화면을 공유하여 프레젠테이션, 문서, 브라우저 창 등을 실시간으로 다른 참가자와 공유.

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

#### 브레인 스토밍

1. 마인드맵

* 실시간 마인드맵 작성 및 공유 기능
  * 웹소켓을   사용해 실시간으로 마인드맵을 작성하고 공유하여 참가자들이 동시에 마인드맵을 확장해 나갈 수 있다.
* 마인드맵 노드 추가, 수정, 삭제 기능
  * 마인드맵의 각 노드를 추가, 수정, 삭제할 수 있으며, 이를 통해 아이디어를 체계적으로 정리할 수 있다.
* 작성된 마인드맵 저장 및 내보내기 기능
  * 파일 시스템 사용

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

2. SWOT

* 강점, 약점, 기회, 위협으로 나누어 아이디어에 대한 생각을 팀원들과 정리하는 기능
* &#x20;웹소켓을 활용하여 실시간으로SWOT의 해당 항목에 팀원들과 낸 아이디어를 정리할 수 있다.

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



3. 여섯가지 사고모자

* 팀원마다 각자 사고하는 방향을 결정하여 역할에 맞게 의논하는 회의 기법
* 웹소켓을 활용하여 실시간으로 채팅을 하여 의견을 타협

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>



4. 크레이지 에이트

* 팀원 모두 8분 안에 8개의 아이디어를 내는 형식으로 팀원들이 낸 각각의 아이디어를 Gemini Ai를 이용하여 요약해 정리한다.
* AWS에 RDS를 사용하여 MySQL에 데이터베이스 관리

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

#### 배포

* AWS에 EC2 기능을 사용하여서 웹 페이지 배포

{% embed url="https://page.teamunmute.com/" %}



### 기술 스택

* 사용 기술
  * React, Spring, AWS, MySQL, Janus Web RTC, WebSocket



* 라이브러리

| 라이브러리         | 설명                        |
| ------------- | ------------------------- |
| Janus Web RTC | 화상회의 서버 라이브러리             |
| React Flow    | 마인드맵 UI 구성 라이브러리          |
| Tailwind CSS  | UI/UX 디자인을 위한 라이브러리       |
| tldraw        | 화이트보드 라이브러리               |
| scrollspy     | 버튼을 눌렸을때 이동을 위한 라이브러리     |
| swiper        | 반응형 슬라이드쇼 및 스와이퍼 기능 라이브러리 |
| Gemini        | 요약기능을 위한 라이브러리            |



### 담당 업무

1. 메인페이지 및 브레인스토밍 설계 및 디자인

* Figma를 사용하여서 사이트 디자인 구성 및 설계
* scrollspy와 swiper를 이용하여 사이트의 편의성과 가독성을 향상



2. 브레인스토밍

* 마인드맵, SWOT, 여섯가지 사고 모자에 실시간 기능을 넣기위해 웹소켓을 사용하여 구현
* 크레이지에이트에 Gemini Ai를 사용하여 다양한 아이디어를 하나의 아이디어로 요약 기능 구현



3. AWS를 사용한 데이터베이스 및 파일 관리

* AWS의 RDS기능을 사용하여 데이터베이스 I/O 관리
* AWS의 S3기능을 사용하여 브레인스토밍의 저장 및 불러오기 기능을 구현
  * JSON형태의 파일로 만들어 관리
* AWS를 사용하기 이전에는 Linux(Ubuntu, CentOS)를 사용하여 데이터베이스 및 화상회의 관리



### 깃허브

{% embed url="https://github.com/YJUCapstoneDesign/ConferenceProject" %}

주요담당 업무 깃허브 경로

* ConferenceProject/FrontEnd/janus-text\_v\_2/src/components
