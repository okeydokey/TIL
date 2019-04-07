# [Spring Cloud Netflix Zuul](<https://cloud.spring.io/spring-cloud-netflix/spring-cloud-netflix.html#_router_and_filter_zuul>)

참고 : [**스프링 마이크로서비스 코딩 공작소**(길벗)](<http://kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791160506815&orderClick=JAj>)



#### 왜 필요한가 ?

- 분산형 아키텍처에서 서비스 호출 사이에 필요한 횡단 관심사를 일관성 있게 구현



#### 기본 개념

- 서비스 클라이언트가 서비스를 직접 호출하지 않고 단일한 정책 시행 지점 역할을 하는 서비스 게이트웨이로 모든 호출을 경유시켜 최종 목적지로 라우팅
- 횡단 관심사
  - 정적 라우팅
  - 동적 라우팅
  - 인증과 인가
  - 측정 지표 수집과 로깅



#### 넷플릭스 주울

##### 주울 라우팅

- 주울은 자동으로 유레카를 사용해 서비스 ID로 서비스를 찾은 후 넷플릭스 리본으로 주울 내부에서 요청에 대한 클라이언트 측 부하 분산 수행

- 서비스 디스커버리를 이용한 자동 경로 매핑

- 서비스 디스커버리를 이용한 수동 경로 매핑

  - ignored-service를 이용해 자동 생성된 유레카 서비스 ID 경로를 제외하고 사용자 정의한 경로만 사용

    ```
    zuul:
      ignore-service: 'organizationservice'
      routes:
      	organizationservice: /organization/**
    ```

  - prefix 프로퍼티를사용해 모든 서비스 경로 앞에 prefix를 추가

- 정적 URL을 이용한 수동 경로 매핑

  - 유레카로 관리하지 않는 서비스를 주울을 사용해 고정 URL에 직접 라우팅

- 경로 구성을 동적으로 로딩

  - /refresh

- 주울과 서비스 타임아웃

  - 히스트릭스는 1초 이상 걸리는 모든 호출을 종료하고 HTTP 500 에러를 반환
    - hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds 히스트릭스 타임아웃 재정의
    - hystrix.command.**servicename**.execution.isolation.thread.timeoutInMilliseconds 서비스에 대해 별도 설정
  - 리본은 5초 이상 수행되는 호출을 타임아웃
  - **servicename**.ribbon.ReadTimeout 리본 타임아웃 재정의

##### 주울 필터

- 게이트웨이를 통과하는 모든 서비스 호출에 대해 사용자 정의 로직을 작성
- 주울 3가지 필터 타입
  - 사전 필터
    - 실제 요청이 발생하기 전에 호출
    - 메시지 형식을 확인하는 작업을 수행, 인증, 인가 등 게이트 키퍼 역할
  - 사후 필터
    - 대상 서비스를 호출하고 응답을 클라이언트로 전송한 후 호출
    - 응답 로깅, 에러 처리, 민감한 정보에 대한 응답을 감시
  - 경로 필터
    - 대상 서비스를 호출하기 전에 호출을 가로채는 데 사용, 동적 라우팅 필요 여부를 결정
    - A/B 테스트

##### (실습) 넷플릭스 주울을 사용해 서비스 게이트 웨이의 구축

- 하나의 URL 뒤에 모든 서비스를 배치하고 서비스 디스커버리를 이용해 모든 호출을 실제 서비스 인스턴스로 매핑
- 서비스 게이트웨이를 경유하는 모든 서비스 호출에 상관관계 ID를 삽입
- 호출할 때 생성된 상관관계 ID를 HTTP 응답에 삽입하고 클라이언트에 회신
- 대중이 사용 중인 것과 다른 조직 서비스 인스턴스 엔드포인트로 라우팅하는 동적 라이팅 매커니즘 구축

