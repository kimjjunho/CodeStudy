## LSP

### LSP : 리스코프 치환 원칙

- 자료형 S가 자료형 T의 하위형이라면 필요한 프로그램의 속성(정확성, 수행하는 업무 등)의 변경 없이 자료형 T의 객체를 자료형 S의 객체로 교환(치환)할 수 있어야 한다는 원칙이다

### 상속 가이드

```설계
Billing ->> License + calcFee() (Interface + method)
Personal License(License 하위 타입) -> License
Business License(License 하위 타입) -> License
```

- 이 설계는 LSP를 준수한다.
  - Billing 애플리케이션의 행위가 License하위 타입 중 무엇을 사용하는 지에 전혀 의존하지 않기 때문
  - 이들 하위 타입은 모두 License타입을 치환할 수 있다

### 정사각형/직사각형 문제

- LSP를 위반하는 전형적인 문제

```설계
User ->> Rectangle + setH + setW
Square + setSide -> Rectangle + setH + setW
```

- 이 예제에서 Square는 Rectangle의 하위 타입으로는 적합하지 않다
  - Rectangle : 높이와 너비가 서로 독립적으로 변경될 수 있다
  - Square : 높이와 너비는 반드시 함께 변경됨
- if문 등을 사용하여 Rectangledl 실제로는 Square인지 검사하는 메커니즘을 User에 추가하면 User의 행위가 사용하는 타입에 의존하게 되므로, 결과 타입을 서로 치환할 수 없게 된다

### LSP와 아키텍처

- LSP는 상속을 사용하도록 가이드하는 방법(초창기) -> LSP는 인터페이스와 구현체에도 적용되는 더 광범위한 소프트웨어 설계 원칙으로 변모
- 자바스러운 언어라면 인터페이스 하나와 이를 구현하는 여러 개의 클래스로 구성된다

### 결론

- LSP는 아키텍처 수준까지 확장할 수 있고, 반드시 확장해야만 한다
- 만약 치환 가능성을 조금이라도 위배하면 상당량의 별도 메커니즘을 추가해야 할 수도 있다

