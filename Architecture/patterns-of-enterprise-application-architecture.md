# [엔터프라이즈 애플리케이션 아키텍처 패턴](http://wikibook.co.kr/peaa/)

### 계층화(layering)
- 장점
  - You can understand a single layer as a coherent whole without knowing much about the other layers.
    다른 레이어에 대해 알지 못해도 하나의 계층을 일관된 전체(?)로 이해할 수 있다.
  - You can substitute layers with alternative implementations of the same basic services.
    같은 기본 서비스의 다른 구현체로 레이어를 대체 할 수 있다.
  - You minimize dependencies between layers.
    레이어들 사이에 의존성을 최소화 할 수 있다.
  - Layers make good places for standardization.
    레이어들은 표준화를 위한 좋은 위치를 만든다.
  - Once you have a layer built, you can use it for many higher-level services.
    한번 구축된 레이어는 많은 상위 수준 서비스에서 사용 할 수 있다.
- 단점
  - Layers encapsulate some, but not all, things well. As a result you sometimes get cascading changes.
    레이어는 일부를 캡슐화 하지만 모두가 잘되진 않는다. 결과적으로 가끔씩 연속적인 변경이 발생한다.
  - Extra layers can harm performance.
    추가 레이어는 성능을 저하시킬 수 있다.