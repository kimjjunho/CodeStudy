# 코루틴

- 비동기적으로 실행되는 코드를 간소화하기 위해 Android에서 사용할 수 있는 동시 실행 설계 패턴이다.
- Android에서 코틀린은 기본 스레드를 차단하여 앱이 응답하지 않게 만들 수도 있는 장기 실행 작없을 관리하는 데 도움이 된다.
- 코루틴은 Android의 비동기 프로그래밍에 권장되는 솔루션이다.

### 기능

1. **경량** : 코루틴을 실행 중인 스레드를 차단하지 않은 **정지**를 지원하므로 단일 스레드에서 많은 코루틴을 실행할 수 있습니다. **정지**는 많은 동시 작업을 지원하면서도 차단보다 메모리를 절약한다.
2. **메모리 누수 감소** : 구조화된 동시 실행{코루틴안에 코루틴을 넣고 결과를 받아서 실행등이 가능함?}을 사용하여 범위 내에서 작업을 실행한다.
3. **기본적으로 제공되는 취소 지원** : 실행 중인 코루틴 계층 구조를 통해 자동으로 **취소**가 전달된다.
4. **Jetpack통합** : 많은 Jetpack라이브러리에 코루틴을 완전히 지원하는 **확장 프로그램**이 포함되어 있다. 일부 라이브러리는 구조화된 동시 실행에 사용할 수 있는 자체 코루틴 범위도 제공

### 예시 개요

- **앱 아키텍처 가이드**에 따라 네트워크 요청을 보내고 결과를 기본 스레드로 반환(앱에서 결과를 사용자에게 표시)
- **ViewModel**아키텍처 구성요소는 기본 스레드의 저장소 레이어를 호출하여 네트워크 요청을 트리거 한다. 이 가이드에서는 코루틴을 사용하는 다양한 솔루션을 반복하여 기본 스레드를 차단 해제 상태로 유지
- **ViewModel**에는 코루틴과 직접 연동되는 KTX 확장 프로그램 집합이 포함된다. 이러한 확장 프로그램은 **lifecycle-viewmodel-ktx**라이브러리이며, 이 가이드에서 사용된다.

### 종속 항목 정보

- build.gradle파일에 다음 종속 항목을 추가

  ```kotlin
  dependencies {
      implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.9")
  }
  ```

### 백그라운드 스레드에서 실행

- 기본 스레드에서 네트워크 요청을 보내면 응답을 받을 때까지 스레드가 대기하거나 차단된다.
- 스레드가 차단되는 경우 이로 인해 OS는 onDraw()를 호출할 수 없으므로 앱이 정지되고 애플리케이션 응답 없음(ANR)대화상자가 표시될 수 있다.

### 예외 처리

Repository레이어에서 발생할 수 있는 예외를 처리하려면 Kotlin에서 기본으로 제공되는 예외 지원을 사용

```kotlin
class LoginViewModel(
    private val loginRepository: LoginRepository
): ViewModel() {

    fun makeLoginRequest(username: String, token: String) {
        viewModelScope.launch {
            val jsonBody = "{ username: \"$username\", token: \"$token\"}"
            val result = try {
                loginRepository.makeLoginRequest(jsonBody)
            } catch(e: Exception) {
                Result.Error(Exception("Network request failed"))
            }
            when (result) {
                is Result.Success<LoginResponse> -> // Happy path
                else -> // Show error in UI
            }
        }
    }
}
```

*위 예시에서는 try-catch블록을 사용

이 예에서는 makeLoginRequest() 호출에 의해 발생한 예기치 않은 예외가 UI에서 오류로 처리된다.





https://developer.android.com/kotlin/coroutines?hl=ko

