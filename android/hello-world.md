# Hello World

문서는 Android Studio를 다루며, 2024년 4월 현재 Iguana (2023.2.1) 버전을 사용하고 있다.

### 새 프로젝트 만들기

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

* Android Studio를 열고, 환경이 로드되면 환영 창에서 "New Project" 버튼을 클릭한다.

<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

1. "New Project" 버튼을 클릭한 후, 새로운 프로젝트 생성 창이 열리면 왼쪽 패널에서 "Empty Views Activity"를 선택한다.
2. 선택한 후, 우측 하단의 "Next" 버튼을 클릭한다.

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

* **Name**: 앱의 이름입니다.
* **Package name**: 앱을 기기 및 플레이 스토어에서 식별하는 데 사용되는 ID입니다.
* **Save location**: 프로젝트를 로컬 저장하는 경로입니다.
* **Language**: 앱을 개발하는 데 사용되는 언어입니다.
* **Minimum SDK**: 앱이 실행되기 위해 필요한 최소한의 안드로이드 SDK 버전입니다.
* **Build configuration language**: 빌드 구성을 설정하는 데 사용되는 언어입니다.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

* 프로젝트 상단에 Android 표시가 나온다면 정상적인 안드로이드 프로젝트로 인식 된 것이다.
  * 프로젝트의 일부 파일이 누락된 경우 표시되지 않는다.
* Android로 출력되는 것은 실제 폴더 구조와는 다른 구조로 표시되는 상태. 주로 이 상태로 개발을 진행한다.
* 실제 폴더 구조가 필요한 경우 Project 로 보기를 전환 해서 사용한다.

### 프로젝트 살펴보기

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

* **MainActivity** : 앱의 핵심이 되는 화면이며, 앱의 주요 동작을 관리하고 사용자와의 상호작용을 조정하는 역할을 한다.
* **소스코드 작업 위치**: 프로젝트를 생성할 때 입력한 패키지명이 기본 패키지로 사용된다.
* **테스트용 패키지 코드**: 테스트용 패키지에 작성한 코드는 실제 앱 동작에는 영향을 주지 않는다.
* **res 폴더**: 각종 디자인 파일을 저장하는 폴더이다.
  * **drawable**: 이미지 파일을 저장한다.
  * **layout**: 화면 디자인 파일을 저장한다.
  * **mipmap**: 해상도별 앱 아이콘 파일을 저장한다.
  * **values**: 색상, 문자열 등의 정적 리소스 파일을 저장한.
* **build.gradle.kts(Project: HelloWorld) :**&#x20;
  * 안드로이드 프로젝트 전체의 빌드 설정을 관리한다.
  * 빌드 스크립트 의존성을 설정하고 필요한 플러그인과 클래스패스를 지정한다.
  * 프로젝트 수준의 빌드 설정을 정의하고, 모든 프로젝트에 적용되는 설정을 지정한다.
  * 빌드 스크립트 플러그인을 적용하여 모듈 수준의 build.gradle.kts 파일에서 사용할 플러그인을 추가한다.
* **build.gradle.kts(Module :app)**
  * 앱 모듈의 빌드 설정을 관리한다.
  * 앱의 의존성을 정의하고, 특정 모듈에 필요한 설정을 지정한다.
  * 안드로이드 애플리케이션의 구성을 설정하고, 빌드 및 패키징에 관련된 정보를 포함한다.

### MainActivity

* HelloWorld 프로젝트의 시작점: onCreate 함수부터 시작

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Android Iguana 부터 MainActivity의 기본 코드가 변경됨.

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

* enableEdgeToEdge
  * 화면이 더 많은 콘텐츠를 표시할 수 있도록 하고자 할 때
  * 툴바, 탭바 등의 UI 요소를 가장자리에 더욱 통합하여 사용자의 작업 영역을 최대화하고자 할 때
  * 전체 화면 모드에서 시스템 UI와 상호 작용을 조정하고자 할 때

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

* ViewCompat.setOnApplyWindowInsetsListener(view) { v, insets -> }&#x20;
* 넓은 화면을 사용할 때 앱 일부가 시스템 아래 그려질 수 있다.
* 표시줄 인셋 val insets = windowInsets.getInsets(WindowInsetsCompat.Type.systemBars())
* 동작 인셋  val insets = windowInsets.getInsets(WindowInsetsCompat.Type.systemGestures())

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

### HelloWorld 출력

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

* xml code를 렌더링해서 출력하게 된다.
