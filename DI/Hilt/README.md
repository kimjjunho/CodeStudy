# Hilt

### 사용 이유

클래스의 종속 항목을 직접 생성, 제공, 관리하는 것을 수동 의존성 주입이라고 한다. 하지만 이 방법은 종속 항목과 클래스가 많아지면 문제가 발생할 수 있다. 따라서 종속 항목을 생성하고 제공하는 프로세스를 자동화해서 문제를 해결하는 라이브러리를 사용하는 것이 좋다. 그중 하나가 안드로이드 주요 DI 라이브러리 Hilt이다.

### @HiltAndroidApp

- 꼭 필요한 어노테이션
- 이후에 activity, fragment, view..에 hilt를 사용할 수 있게된다.

### Container(as hilt-component)

- 각 의존성을 가지고 필요한 곳에 생명주기를 고려하여 의존성 주입을 하는 기능을 가진다.

### @AndroidEntryPoint(component-scope)

- 이 어노테이션이 붙은 안드로이드 클래스는 해당 클래스의 생명주기를 따르는 의존성 컨테이너를 만든다.
- Ex) fragment에서 사용할 경우 이 fragment를 포함하는 activity에도 사용해 주어야 한다.
- @AndoridEntryPoint는 각 안드로이드 클래스와 짝이 맞는 hilt-component를 생성한다
- 컴포넌트는 각 컴포넌트 안에 의존성을 목 주소지로 보낼 수 있는 집합 장소라고 볼 수 있다
  - 컴포넌트(컨테이너) - 물류센터
  - 의존성 - 물류들

### 예외 사항

- 액티비티중 ComponentActivity를 상속하는 액티비티만 지원한다.(AppCompatActivity)
- 프래그먼트중 androidx의 프래그먼트를 상속한 애들만 지원한다.
- Retained-fragment는 지원하지 않는다.(소멸되지 않고 유보된 프레그먼트)

### @Inject

- field injection
- constructor injection

**Fileld Injection**

```kotlin
@AndroidEntryPoint
class LogsFragment : Fragment() {

    @Inject lateinit var logger: LoggerLocalDataSource
    @Inject lateinit var dateFormatter: DateFormatter
    ...
}
```

- hilt가 어떻게 주입해야 하는지 아는 경우 logger, dateFormatter에 의존성을 주입시켜준다.
- private 접근자를 사용하면 hilt는 주입을 하지 못한다.(hilt_field-injection시 private을 사용하면 안된다.)

**Constructor Injection**

1. constructor inject할 수 있는 클래스
   - 내가 구현한 클래스에서 사용가능
   - 해당 구현클래스에 @Inject constructor(...)를 넣어 주면 된다.
2. constructor inject할 수 없는 클래스
   - 인터페이스 or abstract class 구현체, 내가 구현할 수 없는 클래스에서 사용불가
   - 모듈이 필요하다
   - hilt-module : @Module과 @InstallIn 어노테이션을 사용한 클래스
   - @Module : hilt-module임을 가리킴
   - @IntallIn : 어느 안드로이드 클래스(activity, fragment etc)를 사용할건지 가리킴

