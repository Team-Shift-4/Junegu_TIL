# WidgetAndListener2

## 실습 준비

### EmptyActivity 를 가진 새 프로젝트 생성

* Name : Register
* package : com.example.register
* 프로젝트 생성 후 ViewBinding 적용

```kotlin
// MainActivity.kt
package com.example.register

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import com.example.register.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private val binding by lazy { ActivityMainBinding.inflate(layoutInflater) }
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(binding.root)
    }
}
```

```kotlin
// build.gradle.kts(Module:app)
android {
    namespace = "com.example.register"
    compileSdk = 34
    buildFeatures.viewBinding = true
```

## EditText

* 사용자로 부터 문자열을 입력 받을 수 있는 위젯
* 주요 설정 항목
  * android:inputType -> 입력 항목의 출력 및 가상 키보드 종류를 결정
  * android:ems -> layout\_width가 wrap\_content일때 일정 영역을 확보하는 속성. 현재 시스템 폰트 기준으로 대문자 M의 너비를 ems 에 설정된 숫자만큼 확보

### android.text.TextWatcher

* EditText의 입력이 바뀔 때 마다 그 사실을 알려주는 Listener
* 세 가지 함수를 가진다
  * beforeTextChanged(s:CharSequence, start:Int, count:Int, after:Int)
    * s:수정전 문자열, start부터 count개의 글자가 after길이 문자열로 변경된다.
  * onTextChanged(s:CharSequence, start:Int, before:Int, count:Int)
    * s:수정 문자열, start에서 count개의 글자가 before길이의 문자열을 대체한 결과.
  * afterTextChanged(s:Editable)
    * s:수정 완료

### TextWatcher 사용법

* Activity가 직접 구현하는 경우

```kotlin
package com.example.register

import android.os.Bundle
import android.text.TextWatcher
import android.util.Log
import androidx.appcompat.app.AppCompatActivity
import com.example.register.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity(), TextWatcher {
    private val binding by lazy {
        ActivityMainBinding.inflate(layoutInflater)
    }
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(binding.root)
        binding.editTextTextPersonName.addTextChangedListener(this)
    }

    override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {
        Log.d("TextWatcher", "beforeTextChanged ($s, $start, $count, $after)")
    }
    override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {
        Log.d("TextWatcher", "onTextChanged ($s, $start, $before, $count)")
    }
    override fun afterTextChanged(s: android.text.Editable?) {
        Log.d("TextWatcher", "afterTextChanged (${s.toString()})")
    }
}
```

* 각 함수를 파라미터로 넘기는 경우
  * beforeTextChanged와 onTextChanged는 default parameter 가 있으므로 afterTextChanged 만 람다식으로 넘길 수 있다.

```kotlin
package com.example.register

import android.os.Bundle
import android.util.Log
import androidx.appcompat.app.AppCompatActivity
import com.example.register.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private val binding by lazy { ActivityMainBinding.inflate(layoutInflater) }
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(binding.root)
        binding.editTextName.addTextChangedListener {
            Log.d("TextWatcher", "afterTextChanged (${s.toString()})")
        }
    }
}
```

## CompoundButton

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

### CompoundButton.OnCheckedChangeListener

* GUI의 업데이트는 자동으로 이루어진다.
* 기본적으로 onClick과 유사하고 추가정보로 isChecked 를 통해 현재 check 되었는지, 해제되었는지를 알려준다.

<figure><img src="../../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

## RadioButton

* 다른 CompoundButton은 각자 동작
* RadioGroup의 자식으로 RadioButton들을 넣어야 한다.
  * 같은 그룹 내에서 다른 RadioButton이 선택되면 이전 선택이 해제된다.
* RadioGroup 없이 사용할 경우 한 번 클릭하면 선택을 해제할 수 없다.
* RadioGroup은 android:orientation을 이용해 라디오 버튼의 방향을 지정 가능하다.
* 클릭으로 선택을 해제 하는 방법이 없으므로 View.OnClickListener를 사용 가능하다.

<figure><img src="../../.gitbook/assets/image (95).png" alt=""><figcaption></figcaption></figure>

## ImageView

* Bitmap 객체 또는 res / drawable 폴더에 있는 비트맵 형식의 그림(jpg, png) 또는 벡터 형식의 그림(svg를 변환한 xml)을 화면에 출력한다.
  * android:src
* ImageView의 사이즈와 원본 이미지의 사이즈가 다를 수 있으므로 android:scaleType 속성으로 지정한다.

<figure><img src="../../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

### SVG 파일을 프로젝트에 추가하는 방법

* 안드로이드는 벡터 형식의 이미지를 지원한다.
  * 해상도와 상관 없이 깨지지 않고 용량이 적은 것이 장점이다.
* svg파일을 바로 쓸 수는 없고 import 과정을 통해 변환해서 사용한다.
* res / drawable 폴더에서 우클릭 > New > VectorAsset

<figure><img src="../../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

### 벡터 이미지 파일을 프로젝트에 추가하는 방법

* Clip Art : 구글에서 제공하는 벡터 이미지
* Local File : 별도로 다운받아둔 svg나 png 파일을 선택한다.

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

## Progressbar

* 진행을 나타내는 위젯으,로 원형과 바(bar) 타입이 있다.
* 원형을 애니메이션을 가지고 있지만 멈출 수는 없다.
  * android:visibility="visible" 의 속성으로 숨길 수 있다.
* 바 타입은 진행 정도를 원하는대로 적용할 수 있다.
  * android:max(default 100), android:progress
  * Progrssbar: getProgress(), setProgress(Int), incrementProgressBy(Int)

<figure><img src="../../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

## 실습

* 다음과 같은 기능을가지도록 구현하라
* 필수 항목(4개)
  * 이름, 전화번호, 성인/학생, 이용약관&#x20;
* 선택 항목 - 마케팅 활용 동의
* 필수 항목을 채울때 마다 하단 Progressbar는 1/4씩 진행
* Progressbar가 꽉 차기 전에는 버튼이 disable
* Progressbar가 꽉 차면 신청 버튼 Enable
* 중간에 필수 항목의 변동이 생기면 Progressbar에 반영

<figure><img src="../../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

```kotlin
// mainActivity
package com.example.register

import android.os.Bundle
import android.view.View
import androidx.appcompat.app.AppCompatActivity
import androidx.core.widget.addTextChangedListener
import com.example.register.databinding.ActivityMainBinding


class MainActivity : AppCompatActivity() {
    private val binding by lazy { ActivityMainBinding.inflate(layoutInflater) }
    private val check = Array(MAX) { false }
    private val radioListener = View.OnClickListener {
        check[TYPE] = true
        updateProgress()
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(binding.root)
        binding.progressBar.max = MAX
        binding.editTextName.addTextChangedListener {
            check[NAME] = it?.isNotEmpty() == true
            updateProgress()
        }
        binding.editTextPhone.addTextChangedListener {
            check[PHONE] = it?.isNotEmpty() == true
            updateProgress()
        }
        binding.radioButtonAdult.setOnClickListener(radioListener)
        binding.radioButtonStudent.setOnClickListener(radioListener)
        binding.checkBoxService.setOnCheckedChangeListener { _, isChecked ->
            check[AGREEMENT] = isChecked
            updateProgress()
        }
    }

    private fun updateProgress() {
        val progress = check.count { it }
        binding.progressBar.progress = progress
        binding.buttonApply.isEnabled = binding.progressBar.max == progress
    }

    companion object {
        const val NAME = 0
        const val PHONE = 1
        const val TYPE = 2
        const val AGREEMENT = 3
        const val MAX = 4
    }
}
```

```kotlin
// activity_main
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:text="수강신청"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
    <EditText
        android:id="@+id/editTextName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:ems="10"
        android:gravity="center"
        android:hint="이름"
        android:inputType="text"
        android:minHeight="48dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />
    <EditText
        android:id="@+id/editTextPhone"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:ems="10"
        android:gravity="center"
        android:hint="전화번호"
        android:inputType="phone"
        android:minHeight="48dp"
        app:layout_constraintEnd_toEndOf="@+id/editTextName"
        app:layout_constraintStart_toStartOf="@+id/editTextName"
        app:layout_constraintTop_toBottomOf="@+id/editTextName" />

    <RadioGroup
        android:id="@+id/radioGroup"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:orientation="horizontal"
        app:layout_constraintEnd_toEndOf="@+id/editTextName"
        app:layout_constraintStart_toStartOf="@+id/editTextName"
        app:layout_constraintTop_toBottomOf="@+id/editTextPhone" >
        <RadioButton
            android:id="@+id/radioButtonAdult"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="성인반" />
        <RadioButton
            android:id="@+id/radioButtonStudent"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="학생반" />
    </RadioGroup>

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginEnd="8dp"
        app:layout_constraintBottom_toBottomOf="@+id/textView"
        app:layout_constraintEnd_toStartOf="@+id/textView"
        app:layout_constraintTop_toTopOf="@+id/textView"
        app:srcCompat="@drawable/baseline_child_care_24" />
    <TextView
        android:id="@+id/textViewAgreement"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="40dp"
        android:text="약관 동의"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/radioGroup" />
    <CheckBox
        android:id="@+id/checkBoxService"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:text="서비스 이용약관 (필수)"
        app:layout_constraintStart_toStartOf="@+id/editTextName"
        app:layout_constraintTop_toBottomOf="@+id/textViewAgreement" />

    <CheckBox
        android:id="@+id/checkBoxMarketing"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="마케팅 정보 수신동의(선택)"
        app:layout_constraintStart_toStartOf="@+id/checkBoxService"
        app:layout_constraintTop_toBottomOf="@+id/checkBoxService" />
    <ProgressBar
        android:id="@+id/progressBar"
        style="?android:attr/progressBarStyleHorizontal"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="32dp"
        android:layout_marginTop="32dp"
        android:layout_marginEnd="32dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/checkBoxMarketing" />
    <Button
        android:id="@+id/buttonApply"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="24dp"
        android:text="신청"
        android:enabled="false"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/progressBar" />
</androidx.constraintlayout.widget.ConstraintLayout>
```













