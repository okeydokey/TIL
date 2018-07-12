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

#### 도메인 로직 구성
3개의 기본 패턴 : 트랜잭션 스크립트, 도메인 모델, 테이블 모델
##### 트랜잭션 스크립트
작업 또는 비즈니스 트랜잭션마다 스크립트(?) 하나를 만드는 패턴
- 장점
  - 단순한 절차적 모델이다.
  - Row Data Gateway 또는 Table Data Gateway를 사용한 단순한 데이터 소스 레이어와 잘 동작한다.
  - 트랜잭션 경계 설정이 쉽다.
- 단점
  - 도메인 로직가 늘어나면서 복잡도 상승
  - 코드 중복
##### 도메인 모델
도메인에 있는 명사를 바탕으로 모델을 구축한다. 유효성 검사와 계산을 처리하는 논리는 이 도메인 모델에 넣을 수 있다.
##### 테이블 모듈
데이터베이스에 쿼리를 수행해 레코드 집합을 얻고 이 레코드 집합을 인수로 전달해 객체를 만든다.
##### 서비스 계층
도메인 로직를 처리하는 일반적인 방법은 도메인 계층을 둘로 나누는 것이다. 서비스 계층은 명확한 API를 제공한다.
서비스 계층에 얼마나 많은 동작을 넣을지 결정하는 것은 매우 중요하다.

#### 관계형 데이터베이스 매핑
##### Architectural Patterns
- SQL 접근을 도메인 로직과 별도로 분리 하고 데이터베이스 테이블 당 클래스(Gateway) 하나를 가지는 구조
- 게이트웨이를 사용하는 두 가지 방법
  - 쿼리가 반환하는 각 행마다 인스턴스 하나를 만듦(Row Data Gateway)
  - 테이블과 행의 범용 자료구조로서 다양한 환경 지원(Record Set)
- Active Record
  - 도메인 객체가 직접 데이터베이스 로드와 저장을 수행
- 데이터 매퍼
  - 도메인 객체와 데이터베이스의 완전한 격리
- 도메인 로직이 간단할 경우 Active Record, 복잡한 로직이 필요하다면 데이터 매퍼
##### The Behavioral Problem
- 객체가 데이터베이스에 저장 및 로드되는 방법과 동시성의 문제
  - Unit of Work에 커밋을 요청하며 데이터베이스에 수행할 모든 동작을 적절한 순서로 정리한 다음, 커밋을 위한 복잡한 모든 작업을 한곳에서 처리
  - 같은 객체를 두 번 로드하지 않도록 Identity Map에 읽은 모든 행을 기록하여 추적
  - Lazy Load 객체 참조 대신 placeholder를 이용
