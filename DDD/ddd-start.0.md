# [DDD Start!](https://jiandson.co.kr/books/7)

### CH1. 도메인 모델 시작

#### 도메인
- 소프트웨어로 해결하고자하는 문제 영역
- 도메인은 여러 하위 도메인으로 구성

#### 도메인 모델
- 특정 도메인을 개념적으로 표현
- 도메인 자체를 이해하기 위한 개념 모델
- 모델의 각 구성요소는 특정 도메인을 한정할 때 비로소 의미가 완전해지기 때문에, 각 하위 도메인마다 별도로 모델을 만들어야함 (데이터 저장은 ?)

#### 도메인 모델 패턴
- 표현
  - 사용자의 요청을 처리하고 사용자에게 정보를 보여줌
- 응용
  - 사용자가 요청한 *기능*을 실행
  - 업무로직을 직접 구현하지 않으며 도메인 계층을 조합해서 기능을 실행
- 도메인
  - 시스템이 제공할 도메인 규칙을 구현
  - 핵심 규칙을 구현한 코드는 도메인 모델에만 위치하기 때문에 규칙이 바뀌거나 규칙을 확장해야 할 때 다른 코드에 영향을 덜 주고 변경 내역을 모델에 반영 햘 수 있음
- 인프라스트럭처
  - 데이터베이스나 메시징 시스템과 같은 외부 시스템과의 연동 처리

#### 도메인 모델 도출
- 모델을 구성하는 핵심 구성요소, 규칙, 기능
- 요구사항을 부터 시작
- 기능을 메서드로 추가

#### 엔티티와 밸류

##### 엔티티
- 식별자를 갖음
- 엔티티를 생성하고 엔티티의 속성을 바꾸고 엔티티를 삭제할 때까지 식별자는 유지
- 식별자가 같다면 두 엔티티는 같다고 판단

##### 밸류
- 개념적으로 완전한 하나를 표현
  - ex) Receiver(name, phoneNumber), Address(address1, address2, zipcode)
- 의미를 명확하게 표현
  - ex) Money
- 밸류 객체의 데이터를 변경할 때는 기존 데이터를 변경하기보다는 변경한 데이터를 갖는 새로운 밸류 객체를 생성 (불변객체 사용)
- 모든 속성이 같다면 두 엔티티는 같다고 판단

#### 엔티티 식별자와 밸류 타입
- 엔티티의 식별자를 위한 밸류 타입을 사용해서 의미가 잘 드러나도록 함

#### 도메인 모델에 set 메서드 넣지 않기!!
- set메서드는 도메인의 핵심 개념이나 의도를 코드에서 사라지게 함
- 도메인 객체가 불완전한 상태로 사용되는 것을 막으려면 생성 시점에 필요한 것을 전달

#### 도메인 용어
- 가독성
- 알맞은 단어를 찾는 시간을 아까워 하지 말 것!!!



### CH2. 아키텍처 개요

#### 네개의 영역
##### 표현 영역
- HTTP 요청을 응용 영역이 필요로 하는 형식으로 변환
- 응용 서비스가 리턴한 결과를 JSON 형식으로 변환해서 HTTP 응답
##### 응용 영역
- 기능을 구현하기 위해 도메인 영역의 도메인 모델을 사용
- 로직을 직접 수행하기 보다는 도메인 모델에 로직 수행을 위임
##### 도메인 영역
- 도메인 모델을 구현
##### 인프라스트럭처 영역
- 구현 기술 (RDBMS 연동, 메시징 큐에 메시지 전송 및 수신, 몽고DB나 HBase를 사용해서 데이터베이스 연동 처리, SMTP를 이용한 메일 발송, REST API 호출)
##### 도메인 영역, 응용 영역, 표현 영역은 구현 기술을 사용한 코드를 직접 만들지 않고 인프라스트럭처 영역에서 제공하는 기능을 사용해서 필요한 기능을 개발

#### 계층 구조 아키텍처
- 도메인 영역, 응용 영역, 표현 영역은 인프라스트럭처 영역에 의존

#### DIP
- 저수준 모듈이 고수준 모듈에 의존 (추상화 인터페이스)
- 하위 기능을 추상화한 인터페이스는 고수준 모듈 관점에서 도출

#### 도메인 영역의 주요 구성요소
##### 엔티티
- 고유 식별자를 갖는 객체로 자신의 라이프사이클을 갖음
- 도메인의 고유한 개념을 표현
- 도메인 모델의 데이터를 포함하며 해당 데이터와 관련된 기능을 함께 제공
##### 밸류
- 고유 식별자를 갖지 않는 객체로 주로 개념적으로 하나인 도메인 객체의 속성을 표현
##### 애그리거트
- 관련된 엔티티와 밸류 객체를 개념적으로 하나로 묶은 것
##### 리포지터리
- 도메인 모델의 영속성 처리
##### 도메인 서비스
- 특정 엔티티에 속하지 않은 도메인 로직을 제공
- 도메인 로직이 여러 엔티티와 밸류를 필요로 할 경우

##### 엔티티와 벨류
- 도메인 모델의 엔티티는 단순히 데이터를 담고 있는 데이터 구조라기보다는 데이터와 함께 기능을 제공하는 객체, 도메인 관점에서 기능을 구현하고 기능 구현을 캡슐화해서 데이터가 임의로 변경되는 것을 방지
- 도메인 모델의 엔티티는 두 개 이상의 데이터가 개념적으로 하나인 경우 밸류 타입을 이용해서 표현

##### 애그리거트

- 도메인 모델에서 전체 구조를 이해
- 관련 객체를 하나로 묶은 군집
- 군집에 속한 객체들을 관리하는 루트 엔티티를 갖음
  - 루트 엔티티는 애그리거트에 속해 있는 엔티티와 밸류 객체를 이용해서 애그리거트가 구현해야할 기능을 제공
  - 애그리거트를 사용하는 코드는 애그리거트 루트가 제공하는 기능을 실행하고 애그리거트 루트를 통해서 간접적으로 애그리거트 내의 다른 엔티티나 밸류 객체에 접근

##### 리포지터리

- 도메인 객체를 지속적으로 사용하려면 물리적인 저장소에 도메인 객체를 보관
- 구현을 위한 도메인 모델
- 애그리거트 단위로 도메인 객체를 저장

##### 응용 서비스와 리포지터리

- 응용 서비스는 필요한 도메인 객체를 구하거나 저장할 때 리포지터리를 사용
- 응용 서비스는 트랜잭션을 관리하는데, 트랜잭션 처리는 리포지터리의 구현 기술에 영향을 받음
- 리포지터리는 응용 서비스가 필요로 하는 메서드를 제공
  - 애그리거트를 저장한느 메서드
  - 애그리거트 루트 식별자로 애그리거트를 조회하는 메서드

#### 요청 처리 흐름

```sequence
브라우저->Controller: 1.HTTP 요청
Controller->Controller: 1.1 요청 데이터를 응용 서비스에 맞게 변환
Controller->App Service: 1.2 기능 실행
App Service->Repository: 1.2.1 find
Repository-->App Service: 1.2.2 도메인 객체 리턴
App Service->Domain Object: 1.2.3 도메인 로직 실행
App Service-->Controller: 1.2.4 리턴
Controller-->브라우저: 1.2.5 HTTP 응답
```



### CH3. 애그리거트

#### 애그리거트 루트

- 애그리거트의 일관성이 깨지지 않도록 애그리거트가 제공해야 할 도메인 기능을 구현
- 구현시 적용
  - 단순히 필드를 변경하는 set 메서드를 공개 범위로 만들지 않는다.
    - 중요 도메인의 의미가 의도를 포현하지 못하고 도메인 로직이 도메인 객체가 아닌 응용 영역이나 표현 영역으로 분산되게 만든는 원인
    - set 이용시 도메인 로직 분산
  - 밸류 타입은 불변으로 구현한다.


