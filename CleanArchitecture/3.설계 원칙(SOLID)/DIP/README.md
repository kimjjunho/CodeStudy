## DIP

**DIP : 의존성 역전 원칙**

- DIP에서 말하는 '유연성이 극대화된 시스템'이란? 소스 코드 의존성이 추상에 의존하며 구체에는 의존하지 않는 시스템이다.
- 상위 모듈은 하위 모듈에 의존해서는 안된다.
- DIP의 핵심 : 의존 관계를 맺을 때 **변화하기 쉬운 것에 의존하는것보다는, 변화하지 않는 것에 의존**하라는 원칙이다!
- 우리가 의존하지 않도록 피하고자 하는 것은 변경될 일이 적고 의존성을 벗어날 수 없는 구체 클래스가 아닌 변동성이 큰 구체적인 요소이다.

### 안정된 추상화

- 추상 인터페이스에 변경이 생기면 이를 구체화한 구현체들도 따라서 수정해야 한다, 인터페이스는 구현체보다 변동성이 낮다
- 안정된 소프트웨어 아키텍처 원칙
  1. 변동성이 큰 구체 클래스를 참조하지 말라.
     - 언어가 정적 타입이든 동적 타입이든 추상 인터페이스를 참조할 것
  2. 변동성이 큰 구체 클래스로부터 파생하지 말라.
     - 정적 타입 언어던, 동적 타입 언어던 의존성을 가진다는 사실에는 변함이 없어서 신중에 신중을 거듭하는 게 가장 현명한 선택이다.
  3. 구체 함수를 오버라이드 하지 말라.
     - 대체로 구체 함수는 소스 코드 의존성을 필요로 한다
     - 구체 함수를 오버라이드 하면 이런한 의존성을 제거할 수 없게 된다
     - 추상 함수로 선언하고 구현체들에서 각자의 용에 맞게 구현하여 이러한 의존성을 제거해야 한다.
  4. 구체적이며 변동성이 크다면 절대로 그 이름을 언급하지 말라.

### 팩토리

- 변동성이 큰 구체적인 객체는 특별히 주의해서 생성해야 한다

- 모든 언어에서 객체를 생성하려면 해당 객체를 구체적으로 정의한 코드에 대해 소스 코드 의존성이 발생하기 때문이다

- 바람직하지 못한 의존성을 처리할 때 추상 팩토리를 사용한다.

  [![클린아키텍처\] 3부. 설계 원칙 DIP](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSntfXjgae0r63x1ciBp2u81e0b3en8aTVYuQ&usqp=CAU)](https://www.google.com/imgres?imgurl=https%3A%2F%2Fimg1.daumcdn.net%2Fthumb%2FC176x176%2F%3Ffname%3Dhttps%3A%2F%2Fk.kakaocdn.net%2Fdn%2Fb7QJW7%2FbtqYE7Pgv79%2FVcKB4xq1IDUOaQeca48XGK%2Fimg.png&imgrefurl=https%3A%2F%2F32kb.tistory.com%2F39&tbnid=8gcfvlGOu4oGvM&vet=12ahUKEwj5orv11YH3AhVxSfUHHTrWC8kQMyhLegQIARBy..i&docid=shPmnunJhHE4NM&w=176&h=176&q=클린아키택처 팩토리&client=safari&ved=2ahUKEwj5orv11YH3AhVxSfUHHTrWC8kQMyhLegQIARBy)

### 구체 컴포넌트

- 대다수의 시스템은 이러한 구체 컴포넌트를 최소한 하나는 포함할 것이다
- 흔한 이 컴포넌트를 Main이라고 부르는데, main^2 함수를 포함하기 때문이다.



팩토리와 구체 컴포넌트는 클린 아키택처 94p ~ 95p를 참조

[https://ktko.tistory.com/entry/자바-객체-지향의-원리-SOLID-DIP-의존-역전-원칙](https://ktko.tistory.com/entry/자바-객체-지향의-원리-SOLID-DIP-의존-역전-원칙?fbclid=IwAR1wWxFDwu-NtWLK035JUWAebQDi1z3KpJHYwkbRfgMuRgGiAdFzY9qVES8)

### 결론

- 아키텍처 다이어그램에서 가장 눈에 드러나는 원칙이 될 것이다

