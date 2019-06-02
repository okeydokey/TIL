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

### RabbitMQ 란 ?
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

### RabbitMQ 통신 
#### 대화 시작
- AMQP로 통신을 시작할 때, 클라이언트가 프로토콜 헤더를 서버로 전송하여 연결 협상(connection negotiation)을 함
- 클라이언트의 요청을 받은 RabbitMQ는 Connection.Start명령으로 응답해 명령/응답 흐름을 시작하고 클라이언트는 Connection.StartOk 응답 프레임으로 요청에 응답
- 이 과정이 끝나면, RabbitMQ는 애플리케이션의 요청을 받을 준비를 마침

#### 채널
- AMQP에서 채널은 연결 협상이 완료된 AMQP 연결을 정보 전송을 위한 수도관 처럼 사용하고 다른 채널의 대화로부터 전송을 격리 함
- 단일 AMQP 연결에는 여러 채널이 있으므로 클라이언트와 서버 간에 여러 대화를 수행할 수 있음

#### AMQP의 RPC 프레임 구조
- 클래스와 메소드가 있으며 클래스는 기능의 범위를 정의하며 각 클래스에는 서로 다른 작업을 수행하는 메소드가 있음
- AMQP 프레임은 5개의 요소로 구성
  - 프레임 헤더 : 프레임 유형, 채널 번호, 프레임크기(바이트)
  - 프레임 페이로드
  - 끝 바이트 표식(0xce, ASCII값 206)
- AMQP 프레임 5가지 유형
  - 프로토콜 헤더 프레임 : RabbitMQ에 연결할 때 한 번만 사용됨
  - 메소드 프레임 : RabiitMQ와 서로 주고받는 RPC 요청이나 응답을 전달
  - 콘텐츠 헤더 프레임 : 메시지의 크기와 속성을 포함
  - 바디 프레임 : 메시지의 내용을 포함
  - 하트비트 프레임 : RabbitMQ와 연결된 클라이언트와 서버가 주고받으며 서로 사용 가능한 상태인지 확인 하는데 사용
- 메시지를 프레임으로 마샬링하기
  - RabbitMQ에 메시지를 발행할 때 메소드 프레임, 헤더 프레임, 바디 프레임이 사용 됨
  - 첫번째는 명령을 전달하는 메소드 프레임으로 실행하는데 필요한 매개변수인 익스체인지, 라이팅 키를 함게 전송
  - 두번째는 본문 크기와 메시지 속성이 포함되어 있는 콘텐츠 헤더 프레임을 전송
  - 세번째는 바디 프레임으로 AMQP의 최대 프레임 크기가 초과하면 여러 바디 프레임으로 분할되어 전송
  - 전송하는 데이터의 크기를 최소화하기 위해 메소드 프레임과 콘텐츠 헤더 프레임의 내용은 이진 데이터로 구성되어 있음 
  - 바디 프레임의 메시지 내용은 어떤 방식으로도 압축되고나 인코딩되어 있지 않으며 일반 텍스트에서 이진 이미지 데이터에 이르기까지 다양한 형태가 저장 될 수 있음
- 프로토콜 사용
  - 익스체인지 선언
    - Exchange.Declare 명령에 익스체인지의 이름과 유형, 그리고 메시지 처리에 사용하는 기타 메타데이터를 인수로 실행해서 익스체인지를 생성
    - Exchange.Declare 명령을 전송하면 RabbitMQ는 익스체인지를 생성한 후 Exchange.DeclareOk 메소드 프레임을 응답으로 전송, 실패할 경우 숫자 응답 코드와 텍스트 값을 Channel.Close 명령어 포함시켜 전송하고 Exchange.Declare 명령이 전송된 채널을 닫음
  - 큐 선언
    - 익스체인지를 생성한 후 RabbitMQ에 Queue.Declare 명령을 보내 큐를 생성
    - Queue.Declare 명령도 Exchange.Declare 명령과 유사한 통신 절차로 진행되며 Queue.Declare 명령이 실패하면 채널이 닫힘
    - Queue.Declare 명령을 중복 전송 할 경우, RabbitMQ는 중복된 큐 선언을 감지해 큐에 대기 중인 메시지의 수와 구독 중인 쿠독자의 수와 같이 큐에 대한 유용한 상태를 반환
  - 큐와 익스체인지 연결
    - Queue.Bind 명령 사용
  - 메시지 발행
    - 실제 메시지 본문을 RabbitMQ에 전달하기 전에 클라이언트 애플리케이션은 Basic.Publish 메소드 프레임, 콘텐츠 헤더 프레임, 하나 이상의 바디 프레임을 전송
    - 모든 프레임을 수신한 후 RabbitMQ는 다음 단계로 진행하기 전에 메소드 프레임에 포함된 정보를 검증
    - 메소드 프레임의 익스체인지 이름과 일치하는 익스체인지를 발견한 후 해당 익스체인지는 내부 바인딩들을 평가하며 라우팅 키와 일치하는 큐를 찾아 선입선출 순서로 메시지 **참조**를 큐에 삽입
    - 메시지의 모든 사본이 배달되거나 제거돼서 RabbitMQ가 더 이상 메시지를 필요로 하지 않으면, 메시지 데이터의 단일 복사본은 RabbitMQ의 메모리에서 제거
  - 메시지 소비
    - 소비자 애플리케이션은 Basic.Consume 명령을 실행해서 큐를 구독
    - 소비자는 응답받기 적당한 형태인 Basic.Deliver 메소드 프레임과 콘텐츠 헤더 프레임, 바디 프레임으로 메시지를 전달 받음
    - Basic.Consume이 발급되면 특정 상황이 발생하기 전까지 활성 상태를 유지
    - 소비자가 메시지 수신을 중지하려면 Basic.Cancel 명령을 발행, Basic.CancelOk 응답을 받기 전에 RabbitMQ가 미리 할당한 수만큼 메시지를 받을 수 있음(?)
    - Basic.Consume명령의 no_ack 인수를 false로 설정하면 소비자는 ㅎ배달 태그 인수를 포함한 Basic.Ask 응답을 전송

 



### 참고
- [RabbitMQ in Depth](http://acornpub.co.kr/book/rabbitmq-depth)
- [What is a message broker By tibco](https://www.tibco.com/reference-center/what-is-a-message-broker)
- [Java Client API Guide](https://www.rabbitmq.com/api-guide.html)
