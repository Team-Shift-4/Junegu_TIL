# Activity And Intent

## 사용 프로젝트

* Register
  * com.example.register
  * 회원 가입 프로젝트 계속 사용

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## Android Application Component

* 앱을 실행 시키는 진입점
  * 사용자가 앱 아이콘을 클릭, 상단 Notification 클릭, 공유하기 또는 파일 열기 등 앱을 실행 시킬 수 있는 진입점
  * new 연산을 통해 개발자가 직접 객체를 생성할 수 없다. -> 생명 주기를 안드로이드 운영체제가 관리

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### Logcat

* Android 개발 중에 debug를 할 수 있도록 제공
* 사용법
  * Log.i (tag:String?, message:String)
  * i:info, d:debug, w:warning, e:error

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* Lifecycle 사용하지 않은 경우의 코드: MainActivity.kt

















