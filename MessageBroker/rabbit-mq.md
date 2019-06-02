# [RabbitMq](https://www.rabbitmq.com/)

### 메시지 브로커란 ?
- 어플리케이션이 통신 하는 방식이 점차 과제가 되고 있으며 메시지 지향 미들웨어를 이용해 이러한 과제를 단순화함
  - 메시지 지향 미들웨어(MOM, Message-oriented-middleware) : 분산시스템에서 메시지를 보내고 받을 수 있는 소프트웨어 또는 하드웨어 인프라
- 메시지 브로커는 메시지 지향 미들웨어의 빌딩 브록
  - [Message broker vs Message oriented middleware](https://stackoverflow.com/questions/13202200/message-broker-vs-mom-message-oriented-middleware)
  - [Difference between Message Brokers and MOM](https://searchmicroservices.techtarget.com/answer/Difference-between-Message-Brokers-and-MOM)
- 메시지 브로커를 사용하면 source 어플리케이션(producer)이 데이터 마샬링, 라우팅, 메시지 변환, 지속성 및 배달을 모든 적절한 대상(consumers)에게 제공 할 수 있는 서버 프로세스에 메시지를 전달
- Producer와 consumer는 표준 또는 독점적 프로토콜을 사용해 브로커와 통신 함 
- 메시지 브로커는 일반적으로 클라이언트의 모든 상태 관리 및 추적을 제공하므로 개별 어플리케이션이 책임을 맡을 필요가 없으며 메시지 전달의 복잡성이 메시지 브로커 자체에 내장됨
- 메시지 브로커 통신의 두 가지 기본 형식
  - Publish(Publisher) and Subscribe(Consumer) (Topics) : subscriber는 토픽을 구독하고 토픽에 publish된 모든 메시지는 토픽의 모든 가입자가 수신. 하나 이상의 producer가 동일한 주제로 토픽을 publish 할 수 있고 많은 subscriber가 여러 publisher의 메시지를 수신할 수 있음. 이 모델은 애플리케이션이 구독 한 토픽을 기반으로 한 단순한 관심 기반 전달(simple interest driven delivery)을 제공
  - Point-to-Point (Queues) : 가장 단순한 Point-to-point 통신은 하나의 producer 와 하나의 consumer를 가짐. 이 메시징 방식은 대게 큐를 사용하여 consumer가 받을 때까지 producer가 보낸 메시지를 저장 함. 메시지 producer는 큐에 메시지를 보내고 메시지 consumer는 큐에서 메시지를 찾고 수신을 통지 함. 둘 이상의 producer가 메시지를 보낼 수 있으며, 둘 이상의 consumer가 동일한 큐에서 메시지를 소비 할 수 있음. 여러 consumer가 사용될 경우 병렬 처리를 위해 각 consumer는 메시지 스트림의 일부를 수신 

### RabbitMq 란 ?
- 오픈소스 메시지 브로커로 메시지 지향 아키텍처를 구축하기 위한 다양한 기능 제공

### 특징
- 얼랭으로 구현됨
- 얼랭의 IPC(Inter-Process Communication)를 사용해 클러스터링 구현
- AMQP(Advanced Message Queuing Protocol) 스펙 구현
  - AMQ 모델 : 메시지 라우팅 동작을 정의하는 메시지 브로커의 세가지 추상 컴포넌트를 논리적으로 정의
    - 익스체인지 : 메시지 브로커에서 큐에 메시지를 전달하는 컴포넌트
    - 큐 : 메시지를 저장하는 디스크상이나 메모리상의 자료 구조
    - 바인딩 : 익스체인지에 전달된 메시지가 어떤 큐에 저장돼야 하는지 정의하는 컴포넌트
  - RabbitMQ에서의 AMQ
    - 익스체인지 : RabbitMQ에서 메시지를 적절한 목적지로 전달하기 위해 필요한 첫 번째 입력 값, RabbitMQ로 전송한 메시지를 수신하고 메시지를 보낼 위치를 결정. 메시지에 적용할 라우팅 동작을 정의하는데, 일반적으로 메시지를 보낼 때 함께 전달한 데이터 속성을 검사하거나 메시지에 포함된 속성을 이용해 처리
    - 큐 : 수신한 메시지를 저장하는 역할을 하며 메시지에 수행하는 작업을 정의하는 설정 정보를 포함. 큐의 설정 정보에는 메시지를 메모리에만 보관하거나 consumer에게 전달하기 전에 선입 선출 순서로 메시지를 디스크에 보관하는지가 저장 됨
    - 바인딩 : 큐와 익스체인지의 관계를 정의. 익스체인지에 메시지를 발행 할 때 애플리케이션은 라우팅 키 속성을 사용, 바인딩 키는 큐를 익스체인지에 연결하고 라우팅 키를 평가하는 기준. 익스체인트 유형에 따라 라우팅키를 평가하는 방법이 달라지며 다른 속성을 우선적으로 평가해 라우팅 키를 무시하는 유형도 있음

참고
https://www.tibco.com/reference-center/what-is-a-message-broker
