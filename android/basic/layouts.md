# Layouts

## 레이아웃

#### 프로젝트 내의 레이아웃

* Wizard를 이용해 Activity를 생성할 경우 함께 자동으로 생성할 수 있으며 직접 파일을  추가해서 만들어도 된다.
* 런타임에 Activity가 로드하여 사용한다
* res/layout 폴더에 영어 소문자, \_, 숫자 만으로 파일이름을 만든다.
* xml 파일로 작성한다.
  * schema 및 name space 에 대한 선언을 반드시 해야 한다.
  * 해당 내용이 자바 코드로 컴파일 된다 -> xml 파일 내에 오타가 있으면 안된다.
  * 사용하지 않는 파일이 있어도 개발하는데 문제는 없다 -> 나중에 정리 가능하다.



#### xml

* 태그(tag, element)와 속성(attribute)을 사용하는 html 보다 더 넓은 개념
* 임의로 정의한 태그와 속성을 정의해서 사용한다.
  * Schema : 어떤 태그와 속성을 쓸 수 있으며 어떤 hierarchy를 가지는지에 대한 정의
  * Namespace : 특정 태그, 속성의 정의가 어떤 Schema에서 왔는지 알려준다. 태그 이름간의 충돌을 막을 수 있다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
```

* html과 달리 태그는 반드시 닫아야 한다. \<tag>\</tag> 또는 \<tag/>



#### 공통 사항

* 코드와의 연관성
  * 코드 상의 클래스 이름 -> Tag
  * 코드 상의 클래스 멤버(property) -> Attribute
    * Attribute는 namespace를 정확히 적어준다.

```xml
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
```

* Activity에서 불러 사용하는 레이아웃 파일의 최상위 항목은 반드시 레이아웃이여야 한다.
  * Layout은 내부에 Layout을 중첩할 수 있다.
  * Layout은 내부에 여러 개의 Widget을 가질 수 있다.
* Android SDK에 포함된 Layout, Widget은 Class Name만 적고 사용
  * android.widget.LinearLayout
  * Linear layout, Relative layout, Frame layout, Grid layout 등
* 그 외 Layout Widget은 package 를 포함하는 Fully-qualified Class Name을 사용
  * androidx.constraintlayout.widget.ConstraintLayout



#### 주요 Hierarchy

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>



### 공통 속성



#### 정렬, 사이즈

* Left/Start, Right/End
  * Widget, Layout의 위치나 정렬을 지정할 때 사용하는 표현
  * Start - 현재 언어의 시작 방향 (한국어, 영어는 Start=Left)
  * 낮은 minimum SDK를 지정했을 경우 Left/Right 표현을 함께 사용해야 한다.
* android:layout\_width, android:layout\_height
  * Widget, Layout의 사이즈를 지정하는 필수 속성.
  * match\_parent, wrap\_content, dp 사이즈 직접 입력 등의 방법이 있다.



#### 여백

* 주요 설정 항목
  * android:layout\_margin : 부모나 다른 widget과의 간격
  * android:padding : 자신의 내부 자식 요소와의 간격
  * 주로 dp로 지정한다.
* Top/Start(Left)/Bottom/End(Right) 각각 지정할 수도 있다
  * ex) android:paddingTop

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>



#### 식별자

* android:id
  * 레이아웃에서 다른 Widget/Layout을 참조할 때
  * Code 에서 해당 Widget/Layout을 사용해야 할 때 사용할 식별자
  * 일반적인 변수 Naming convension을 따르면 편하다.
  * 새 아이디를 부여하려면 @+id/아이디
  * 이미 만들어진 아이디를 참조할 때는 @id/아이디
    * xml 파일은 윙서 아래로 해석되므로 줄번호가 아래쪽에 있는 아이디는 @id/아이디 로 참조하지 못한다. 이 경우 @+id/아이디 로 참조해야 한다.



### Linear Layout



### 새 파일 추가

* app/res/layout 폴더에서 우클릭, New > Layout Resource File

<figure><img src="../../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

* File name : activity\_main\_linear
* Root element : LinearLayout

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

* 이데터 우측 상단 모드 중 편한 화면을 선택. xml은 Code 또는 Split에서 볼 수 있다.

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

* 현재는 빈 화면 상태
  * inset 을 적용하기위해 아이디를 부여함
  * 필수 속성 : vertical 또는 horizontal

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

* MainActivity.kt 를 다음과 같이 수정 후 앱을 실행하여 빈 화면이 나오는지 확인한다.

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>



### 레이아웃에 위젯 추가

* Design 보기에서 드래그 & 드롭으로 추가 후 우측 속성 화면에서 편집.
* 미세한 조정은 Code 또는 Split에서 코드 단위로 수정

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

* Palette에서 Button 항목을 4번 드래그 하여 다음과 같이 만들어본다.

<figure><img src="../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

### 속성 변경

* 각 버튼의 android:layout\_width를 wrap\_content로 변경해본다.
  * Design 보기의 우측 속성 변경 또는&#x20;
  * Code상에서 수정

<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

* 각 버튼의 아이디를 다음과 같이 수정하고 Text 속성을 변경하여 출력되는 버튼의 라벨을 변경하라.
  * buttonA, buttonB, buttonC, buttonD

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

### 레이아웃 속성 변경

* 레이아웃의 속성 중 orientation을 horizontal 로 변경해본다.

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

### 실습

* LinearLayout과 Button만을 사용하여 다음과 같은 화면을 구성하라

<figure><img src="../../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

* LinearLayout과 Button만을 사용하여 다음과 같은 화면을 구성하라

<figure><img src="../../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

## Constraint Layout

### 기본 정보

* 기존 레이아웃들의 불편한 점들을 개선한 레이아웃
* 기본 라이브러리에는 포함되어 있지 않으며 Module의 build.gradle의 dependencies 항목과 libs.versions.toml 다음과 같이 의존성이 추가되어야 사용 가능

<figure><img src="../../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

* Iguana 이전 버전의 프로젝트는 Module의 build.gradle에 라이브러리 이름과 버전이 함께 기술된다.



* 기존 레이아웃들의 불편한 점들을 개선한 레이아웃
* 각 View는 상/하/좌/우 (Top, Bottom, Left, Right)에 제약점(Constraint)을 가지며 이 제약점을 부모나 다른 View에 연결하여 자신의 위치를 결정한다.

<figure><img src="../../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

* 각 View는 반드시 수평, 수직 방향의 제약점이 각각 하나 이상 연결되어야 한다.
  * 각 View를 연결하는 연결선은 스프링과 같이 동작한다.(View를 잡아당긴다.)

<figure><img src="../../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>

* app:layout\_constraintLeft\_toLeftOf="parent" 또는 app:layout\_constraintLeft\_toLeftOf="@ +id/other" 와 같은 형식으로 대상 지정 자신의 제약점\_대상의제약점 형식으로 제약점 연결



* 우측 속성 창에서 수평, 수직의 힘의 비율(bias)을 조절할 수 있으며 amrgin도 설정할 수 있다.

<figure><img src="../../.gitbook/assets/image (82).png" alt=""><figcaption></figcaption></figure>

### 실습

* HelloWorld를 전체 화면의 위쪽 30%위치, 가로는 가운데 정렬되도록 위치를 수정하라.

<figure><img src="../../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>

* Hello World 문구 하단에 Button을 추가한다.
* Button은 Hello World와 20dp의 간격을 두고 위치하며 가로로는 가운데 정렬한다.

<figure><img src="../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

* Hello World의 가로정렬도 왼쪽으로 0.3 위치에 배치한다
* Button은 세로 정렬은 유지하고 왼쪽 시작 부분을 HelloWorld와 맞춘다

<figure><img src="../../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

* activity\_main.xml에서 constraint layout을 이용해 다음과 같이 위젯들을 배치하고 앱 실행 시 이 화면이 출력되도록 수정하라.
* 글자를 입력할 수 있는 위젯은 Palette의 Text 분류에 있으며 그 중 Number를 사용한다
* 각 위젯의 아이디는 다음과 같다

<figure><img src="../../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>
