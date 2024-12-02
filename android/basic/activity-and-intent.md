# Activity And Intent

## 사용 프로젝트

* Register
  * com.example.register
  * 회원 가입 프로젝트 계속 사용

<figure><img src="../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Android Application Component

* 앱을 실행 시키는 진입점
  * 사용자가 앱 아이콘을 클릭, 상단 Notification 클릭, 공유하기 또는 파일 열기 등 앱을 실행 시킬 수 있는 진입점
  * new 연산을 통해 개발자가 직접 객체를 생성할 수 없다. -> 생명 주기를 안드로이드 운영체제가 관리

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 앱이 가진 Component의 정보를 안드로이드 시스템이 알아야 한다.
  * AndroidManifest.xml
* Activity
  * GUI를 제공하며 사용자와 상호작용하는 Component
* Service
  * 화면 없이 백그라운드에서 작업을 하는 컴포넌트
* Content Provider
  * 앱이 가진 데이터를 공유하는 컴포넌트, 다른 앱이 내 앱의 정보를 사용할 수 있게 해준다.
* Broadcast Receiver
  * 시스템이 발생시킨 이벤트를 받을 수 있는 컴포넌트.

## Activity Lifecycle

* onCreate(필수 override 함수)
  * Activity가 생성될 때
* onStart
  * Activity가 사용자에게 보일 때
* onResume
  * 사용자와 상호작용이 가능할 때
* 반대 순서
  * onPause -> onStop -> onDestroy

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Logcat

* Android 개발 중에 debug를 할 수 있도록 제공
* 사용법
  * Log.i (tag:String?, message:String)
  * i:info, d:debug, w:warning, e:error

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Lifecycle 확인

* onCreate 외에 onStart, onResume을 override 하여 logcat을 통해 라이프 싸이클을 확인한다.

```kts
package com.example.register

import android.os.Bundle
import android.util.Log
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

        Log.i("MainActivity", "onCreate")
    }
    override fun onStart(){
        super.onStart()
        Log.i("MainActivity", "onStart")
    }
    override fun onResume() {
        super.onResume()
        Log.i("MainActivity", "onResume")
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

* Activity의 Lifecycle에 맞추어 동작해야 하는 기능이 있는 경우 (카메라, 위치 확인 등) 각 lifecycle 함수에서 해당 기능을 초기화, 멈춤, 해제 등의 함수를 호출해야 한다.
* 문제점
  * 각 lifecycle 함수가 복잡해진다.
  * 해당 기능의 활성화에 지연이 발생할 경우 Activity가 stop되고 난 후 callback이 실행될 수도 있다. -> 호출 이후 Activity의 상태와 상관 없이 코드가 실행된다.

### Lifecycle Observer

* Lifecycle
  * Activity, Fragment 등과 같은 LifecycleOwner의 Lifecycle 상태에 대한 정보를 다른 객체가 관찰할 수 있도록 하는 클래스

<figure><img src="../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Lifecycle 사용하지 않은 경우의 코드: MainActivity.kt

```kotlin
package com.example.register

import android.os.Bundle
import android.util.Log
import android.view.View
import androidx.appcompat.app.AppCompatActivity
import androidx.core.widget.addTextChangedListener
import com.example.register.databinding.ActivityMainBinding

class MyCycle {
    fun start() {
        Log.i("MainActivity", "MyCycle start")
    }

    fun stop() {
        Log.i("MainActivity", "MyCycle stop")
    }

    class MainActivity : AppCompatActivity() {
        private lateinit var myCycle: MyCycle

        private val binding by lazy { ActivityMainBinding.inflate(layoutInflater) }
        private val check = Array(MAX) { false }
        private val radioListener = View.OnClickListener {
            check[TYPE] = true
            updateProgress()
        }

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(binding.root)

            myCycle = MyCycle()
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

        override fun onStart() {
            super.onStart()
            Log.i("MainActivity", "onStart")
            myCycle.start()
        }

        override fun onResume() {
            super.onResume()
            Log.i("MainActivity", "onResume")

        }

        override fun onStop() {
            super.onStop()
            Log.i("MainActivity", "onStop")
            myCycle.stop()
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
}
```

* Lifecycle 사용하기 - libs.versions.toml 수정 및 sync

```gradle
[versions]
agp = "8.3.1"
kotlin = "1.9.0"
coreKtx = "1.13.0"
junit = "4.13.2"
junitVersion = "1.1.5"
espressoCore = "3.5.1"
appcompat = "1.6.1"
material = "1.11.0"
activity = "1.8.0"
constraintlayout = "2.1.4"
lifecycle = "2.7.0"

[libraries]
androidx-core-ktx = { group = "androidx.core", name = "core-ktx", version.ref = "coreKtx" }
junit = { group = "junit", name = "junit", version.ref = "junit" }
androidx-junit = { group = "androidx.test.ext", name = "junit", version.ref = "junitVersion" }
androidx-espresso-core = { group = "androidx.test.espresso", name = "espresso-core", version.ref = "espressoCore" }
androidx-appcompat = { group = "androidx.appcompat", name = "appcompat", version.ref = "appcompat" }
material = { group = "com.google.android.material", name = "material", version.ref = "material" }
androidx-activity = { group = "androidx.activity", name = "activity", version.ref = "activity" }
androidx-constraintlayout = { group = "androidx.constraintlayout", name = "constraintlayout", version.ref = "constraintlayout" }
androidx-lifecycle-runtime-ktx = {group = "androidx.lifecycle", name="lifecycle-runtime-ktx", version.ref = "lifecycle"}

[plugins]
androidApplication = { id = "com.android.application", version.ref = "agp" }
jetbrainsKotlinAndroid = { id = "org.jetbrains.kotlin.android", version.ref = "kotlin" }


```

* Lifecycle 사용하기 build.gradle.kts(Module:app) 수정 및 sync

```gradle
dependencies {

    implementation(libs.androidx.core.ktx)
    implementation(libs.androidx.appcompat)
    implementation(libs.material)
    implementation(libs.androidx.activity)
    implementation(libs.androidx.constraintlayout)
    // lifecycle
    implementation(libs.androidx.lifecycle.runtime.ktx)
    testImplementation(libs.junit)
    androidTestImplementation(libs.androidx.junit)
    androidTestImplementation(libs.androidx.espresso.core)

}
```

* Lifecycle을 참조하는 경우: MainActivity.kt

```kotlin
package com.example.register

import android.os.Bundle
import android.util.Log
import android.view.View
import androidx.appcompat.app.AppCompatActivity
import androidx.core.widget.addTextChangedListener
import androidx.lifecycle.DefaultLifecycleObserver
import androidx.lifecycle.LifecycleOwner
import com.example.register.databinding.ActivityMainBinding

class MyCycle: DefaultLifecycleObserver{
    override fun onStart(owner: LifecycleOwner) {
        Log.i("MyCycle", "onStart")
    }

    override fun onStop(owner: LifecycleOwner) {
        Log.i("MyCycle", "onStop")
    }
}

class MainActivity : AppCompatActivity() {
    private lateinit var myCycle: MyCycle
    private val binding by lazy { ActivityMainBinding.inflate(layoutInflater) }
    private val check = Array(MAX) { false }
    private val radioListener = View.OnClickListener {
        check[TYPE] = true
        updateProgress()
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(binding.root)

        myCycle = MyCycle()
        lifecycle.addObserver(myCycle)

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

## Intent

* Message를 주고 받기 위한 객체
* Application component를 요청: Activity, Service의 시작. Broadcast 전달
* 명시적 Intent와 암시적 Intent가 있다.
  * 명시적 Intent: fully qualified class name을 제공 -> 내 앱 안에 Activity 또는 Service 시작
  * 암시적 Intent: 필요한 작업(동작)을 선언. 해당(기능을A지원(AndroidManifest.xml 에을선언된   Activity에지자신이원처리할할수 있는수일을 적는다) 있는 앱을 선택할 수 있도록 한다.

<figure><img src="../../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

### 명시적 Intent

* 프로젝트에 Activity 추가하기
  * MainActivity가 포함된 패키지에서 우클릭
  * New
  * Activity
  * EmptyActivity

<figure><img src="../../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

* Activity Name: CourseActivity&#x20;
* Generate a Layout File에 체크 > Finish

<figure><img src="../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

* 확인
  * CourseActivity.kt 생성
  * res / layout/ activity\_course.xml 생성
  * AndroidManifest.xml에 정보 추가
* MainActivity에서 CourseActivity를 부르는 방법:
  * MainActivity의 onCreate 함수 끝 부분에 다음 코드 추가

```kotlin
binding.buttonApply.setOnClickListener {
            val intent = Intent(this, CourseActivity::class.java)
            startActivity(intent)
        }
```

### Activity Stack

* 생성된 Activity는 Stack에 쌓으며 가장 위의 Activity가 보인다.
* Back 키를 누르거나 Activity 스스로 finish() 함수를 호출하면 종료된다.

<figure><img src="../../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

* Activity는 싱글톤이 아니며 startActivity를 호출한 횟수만큼 생성된다.

### Activity 간의 데이터 전달

* 단방향: 호출 하는 Activity가 데이터 함께 전달하기
* MainActivity에서 사용자 이름, 분반(성인반/학생반)을 CourseActivity로 전달
* MainActivity

```kotlin
binding.buttonApply.setOnClickListener {
            val type = if(binding.radioButtonAdult.isChecked) binding.radioButtonAdult.text
            else binding.radioButtonStudent.text
            val intent = Intent(this, CourseActivity::class.java)
            intent.putExtra("name", binding.editTextName.text.toString())
            intent.putExtra("type", type)

            startActivity(intent)
        }
```

intent.putExtra(name, object) 형식으로 데이터를 담는다.

* CourseActivity에서는 받은 데이터 꺼내기

```kotlin
class CourseActivity : AppCompatActivity() {
 private val binding by lazy { ActivityCourseBinding.inflate(layoutInflater) }
 override fun onCreate(savedInstanceState: Bundle?) {
 super.onCreate(savedInstanceState)
 enableEdgeToEdge()
 setContentView(binding.root)
 ViewCompat.setOnApplyWindowInsetsListener(binding.main) { v, insets ->
 val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
 v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
 insets
 }
 binding.textViewName.text = intent.getStringExtra("name")
 binding.textViewType.text = intent.getStringExtra("type")
 }
}
```











