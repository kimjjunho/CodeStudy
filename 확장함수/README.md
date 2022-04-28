# 확장함수

- 코틀린이나 자바에서 '상속'을 통해 SuperClass(부모 클래스)를 확장할 수도 있지만 코틀린에서 지원하는 클래스 확장 개념은 다른 방법으로 클래스를 확장한다.

- 코틀린에서 확장함수는 기본 클래스에 정의된 함수인 것처럼 새로운 함수를 추가하는 기능이다.

- 코틀린에서는 함수를 클래스 안에 선언하지 않아도 된다.

- 확장 함수는 static함수이며, 클래스 밖에 선언되기 때문에 오버라이드 할 수 없다.

  [확장 함수 생성 문법]

  ~~~kotlin
  fun 확장할 클래스.함수명: 리턴타임{
  		return 리턴 값
  }
  ~~~

- 어떠한 존재하는 클래스가 존재하는데 이 클래스를 직접 수정할 수 없을 때 기존 클래스 주변에 새로운 함수나 프로퍼티를 추가하여 클래스의 크기를 늘린다(확장한다)고 생각하면 됨

  ![01](https://user-images.githubusercontent.com/31889335/102179222-20e3c700-3eea-11eb-9939-89c47ab447ec.png)



### 확장 함수 만드는 방법

- **receiver type(수취인 타입)**
  1. 확장 함수를 추가할 클래스를 말한다.
  2. 확장 대상이 될 클래스이다.
- **receiver object(수취인 객체)**
  1. 확장 함수 내부를 구현할 때 this 키워드를 사용하여 receiver type이 가지고 있는 public 인스턴스에 접근할 수 있다.
  2. 이렇게 접근한 객체를 receiver object라고 부른다.



### 확장 함수가 갖는 특징

1. **확장 함수는 정적 바인딩된다.**

   *함수 바인딩 : 함수를 만들어 컴파일하면 함수의 코드가 메모리 어딘가에 저장되고, 함수를 호출하는 부분에는 해당 함수의 코드가 저장된 메모리 주소 값이 저장된다.

   *프로세서는 함수 호출 부분에 저장된 메모리 주소를 보고 메모리 주소에 있는 코드를 실행하게된다.

   **함수 바인딩의 두 종류**

   - 정적 바인딩 : 함수 호출 부분에 메모지 주소 값을 저장하는 작업이 <u>컴파일 시간</u>에 행해지기 때문에 컴파일 이후로 이 값이 변경되지 않는다.
   - 동적 바인딩 : 함수 호출 부분에 메모리 주소 값을 저장하는 작업이 컴파일 시간에는 보류되고, **런타임**에 결정된다

   [이해를 위한 코드]

   ```kotlin
   open class Shape
   
   //Rectangle = Shape 클래스를 상속하는 SubClass
   class Rectangle: Shape()
   
   //Shape 클래스의 확장 함수
   fun Shape.getName() = "Shape"
   
   //Rectangle 클래스의 확장 함수
   fun Rectangle.getName() = "Rectangle"
   
   fun printClassName(s:Shape){
     println(s.getName())
   }
   
   //매개 변수의 타입이 Shaoe인 함수에 Rectangle 타입의 인스턴스를 전달함
   printClassName(Rectangle())
   ```

   printClassName() 매개변수 타입 : Shape

   printClassName()을 호출할 때는 printClassName(Rectangle())이라고 호출

   즉 Rectangle타입의 인스턴스를 전달한 것

   하지만 메모리 주소가 이미 컴파일 시간에 결정되어 버렸다

   ```kotlin
   // 확장 함수를 호출하는 부분
    fun printClassName(s: Shape) {
        println(s.getName())
    }
   ```

   위 코드가 컴파일될 때, 확장 함수의 호출 부분인 s.getName()부분에는 이미 Shape 클래스의 확장 함수인 getName()함수가 저장된 메모리 주소가 저장되어 있는 것이다.

   따라서 위 코드의 실행 결과는 "Shape"이 출력되게 된다.

2. **확장 함수와 이름 및 매개 변수 타입, 매개 변수 개수가 완벽히 같은 함수가 확장 대상 클래스의 멤버 함수로 존재하면 확장 함수는 무시된다.**

   - 확장 대상 클래스에 이미 존재하는 멤버 함수와 이름도 똑같고, 매개변수도 똑같은 확장 함수를 만들면 확장 함수를 호출해도 멤버 함수가 호출된다.(확장 함수가 무시됨)
   - 하지만 이름이 같아도 매개변수의 type이나 매개 변수 개수가 달라 확장 함수와 멤버 함수를 구분할 수 있다면 원하는 함수를 호출할 수 있다.

   ```kotlin
   fun main() {
        Test().solution('c')
        Test().solution(1)
        Test().solution2(3) // 확장 함수가 호출되지 않고, 멤버 함수가 호출된다.
    }
   
    class Test {
        fun solution(char: Char) {
            println("안녕")
        }
   
        fun solution2(int: Int) {
            println("반가워")
        }
    }
   
    // 확장 함수 - Test 클래스 안의 멤버 함수인 solution()과 이름은 같지만 매개 변수 타입이 달라 구분 가능한 함수
    fun Test.solution(int: Int) {
        println("잘가")
    }
   
    // 확장 함수 - Test 클래스 안의 멤버 함수인 solution2()와 이름도 같고, 매개 변수 타입도 같아서 구분이 불가능한 함수
    fun Test.solution2(int: Int) {
        println("또봐")
    }
   ```

   ```실행결과
   하이
   안뇽
   반가워
   ```

   확장 함수는 멤버 함수를 오버로딩할 수는 있다.



### 예시

```kotlin
fun main(){
	val source = "Hello World!"
	val target = "Kotlin"
	println(source.getLongString(target))
}

//String을 확장해 getLongString추가
fun String.getLongString(target: String): String =
if(this.length > target.length) this else target
```

```실행결과
Hello World!
```



### 구글링

https://ddolcat.tistory.com/558

https://choheeis.github.io/newblog//articles/2020-12/kotlin-extension-function