# [엔터프라이즈 애플리케이션 아키텍처 패턴](http://wikibook.co.kr/peaa/)

#### 계층화(layering)
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

#### 도메인 논리 구성
3개의 기본 패턴 : 트랜잭션 스크립트, 도메인 모델, 테이블 모델
##### 트랜잭션 스크립트
작업 또는 비즈니스 트랜잭션마다 스크립트(?) 하나를 만드는 패턴
- 장점
  - 단순한 절차적 모델이다.
  - Row Data Gateway 또는 Table Data Gateway를 사용한 단순한 데이터 소스 레이어와 잘 동작한다.
  - 트랜잭션 경계 설정이 쉽다.
- 단점
  - 도메인 논리가 늘어나면서 복잡도 상승
  - 코드 중복
##### 도메인 모델
도메인에 있는 명사를 바탕으로 모델을 구축한다. 유효성 검사와 계산을 처리하는 논리는 이 도메인 모델에 넣을 수 있다.
##### 테이블 모듈
데이터베이스에 쿼리를 수행해 레코드 집합을 얻고 이 레코드 집합을 인수로 전달해 객체를 만든다.
##### 서비스 계층
도메인 논리를 처리하는 일반적인 방법은 도메인 계층을 둘로 나누는 것이다. 서비스 계층은 명확한 API를 제공한다.
서비스 계층에 얼마나 많은 동작을 넣을지 결정하는 것은 매우 중요하다.
