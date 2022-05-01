

### SAP : 안정된 추상화 원칙

- 컴포넌트는 안정된 정도만큼만 추상화되어야 한다.

**고수준 정책을 어디에 위치시켜야 하는가?**

- 고수준 아키텍처나 정책 결정과 관련된 소프트웨어는 자주 변경해서는 절대로 안된다.
- 고수준 정책을 캡슐화하는 소프트웨어 - 안정된 컴포넌트(I=0)에 위치해야 한다
- 변동성이 큰 소프트웨어 - 불안정한 컴포넌트(I=1)에 포함되어야 한다
- 고수준 정책을 안정된 컴포넌트에 위치시키면 -> 그 정책을 포함하는 소스 코드는 수정하기 어려워진다
- 컴포넌트가 안정된 상태로 변경에 충분히 대응할 수 있으려면 -> 개방 폐쇄 원칙(OCP) 사용 -> <u>추상 클래스</u>가 이러한 원칙을 준수 한다

**안정된 추상화 원칙**

- 안정된 추상화 원칙은 안정성과 추상화 정도 사이의 관계를 정의한다.
- 안정된 커포넌트는 추상 컴포넌트여야 하며, 안정성이 컴포넌트를 확장하는 일을 방해해서는 안된다.
- 불안정한 컴포넌트는 반드시 구체 컴포넌트여야 한다.
- SAP + SDP = DIP나 마찬가지.
- 하지만 SDP와 SAP의 조합은 컴포넌트에 대한 원칙이고, DIP원칙이다.
- 클래스 : 추상적이거나 아니거나, 둘 중 하나
- 컴포넌트 : 어떤 부분은 추상적이면서 다른 부분은 안정적일 수 있다

**추상화 정도 측정하기**

- A지표는 컴포넌트의 추상화 정도를 측정한 값이다. 클래스 총 수 대비 인터페이스와 추상 클래스의 개수를 단순히 계산한 값.
  - Nc: 컴포넌트의 클래스 개수
  - Na: 컴포넌트의 추상 클래스와 인터페이스의 개수
  - A: 추상화 정도. A = Na / Nc
- A는 0~1사이이의 값을 갖는다.
  - A = 0 : 컴포넌트에 추상 클래스가 하나도 없다
  - A = 1 : 컴포넌트에 오직 추상 클래스만을 포함한다.

**주계열**

- 안정성 : I
- 추상화 정도 : A
- 수직축에 A를 수평축에 I를 나타내는 그래프를 그려보면 최고로 안정적이며 추상화된 컴포넌터는 좌측 상단(0,1)에 위치하고 최고로 불아정하며 구체화된 컴포넌트는 우측 하단인(1,0)에 위치한다
- 대체로 추상화와 안정화의 정도가 다양하기 때문에 이 두 지점에만 위치하는 것은 당연히 아니다
- 주계열은 이 그래프를 대각선으로 나눈다
- 고통의 구역(왼쪽 아래) / 주계열 / 쓸모없는 구역(쓸모없는 구역)

고통의 구역

- (0,0)주변에 위치한 컴포넌트로 매우 안정적이며 구체적이고 뻣뻣한 상태이기에 바람직한 상태가 아니다.
- 데이터베이스 스키마 같은 일부 소프트웨어 엔티티가 고통의 구역에 위치한다.

쓸모없는 구역

- (1,1)주변에 컴포넌트로 최고로 추상적이지만 누구도 의존하지 않아서 쓸모가 없다.
- 이 구역에 존재하는 엔티티는 폐기물과도 같다

![Clean Architecture 1/2](data:image/png;base64,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)

**배제 구역 벗어나기**

- 변동성이 큰 컴포넌트 대부분은 두 배제 구역으로부터 가능한 멀리 떨어뜨려야 한다
- 주계열의 정의 : 각 배제 구역으로 부터 최대한 멀리 떨어진 점의 궤적 + (1,0)과 (0,1)을 잇는 선분
- 주계열에 위치한 컴포넌트는 너무 추상적이지도 너무 불안정하지도 않기 때문에 컴포넌트는 주계열 근처에 위치하는 것이 가장 이상적이다.

**주계열과의 거리**

이상적인 상태로부터 컴포넌트가 얼마나 멀리 떨어져 있는지 측정하는 지표

- D^4 : 거리.D=|A + I - 1|.
  - 유효범위 : [0,1]
  - D가 0이면 컴포넌트가 주계열 바로 위에 위치한다는 뜻
  - 1이면 주계열로부터 가장 멀리 위치한다는 뜻

1. 각 컴포넌트에 대한 D지표 계산
2. 설계를 통계적으로 분석할 수 있음

**결론**

- 의존성 관리 지표는 설계의 의존성과 추상화 정도가 '훌륭한'패턴이라고 생각되는 수준에 얼마나 잘 부합하는지 측정한다
- 이 지표 역시 아무리 해도 불완전하지만 지표로 부터 무언가 유용한 것을 찾을 수 있다

