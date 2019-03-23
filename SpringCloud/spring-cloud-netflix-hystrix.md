# [Spring Cloud Netflix Hystrix](<https://cloud.spring.io/spring-cloud-netflix/spring-cloud-netflix.html#_circuit_breaker_hystrix_clients>)

참고 : [**스프링 마이크로서비스 코딩 공작소**(길벗)](<http://kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791160506815&orderClick=JAj>)

#### [설정](<https://github.com/Netflix/Hystrix/wiki/Configuration>)

- fullbackMethod : 히스트릭스에서 호출이 실패할 때 불러오는 클래스 함수를 정의
- threadPoolKey : 스레드 풀의 고유 이름 정의
- threadPoolProperties : threadPool 동작 정의
  - coreSize : 스레드 풀의 스레드 개수
  - maxQueueSize : 스레드 풀 앞에 배치할 큐와 큐에 넣을 요청 수

- commandProperties 
  - execution.isolation.thread.timeoutInMilliseconds : 히스트릭스 호출이 실패하기 전까지 대기할 최대 타임아웃 시간
  - circuitBreaker.requestVolumeThreshold : 회로 차단기의 차단 여부를 검토하는 데 필요한 최소 요청 수 (10초 간격)
  - circuitBreaker.errorThreasholdPercentage : 회로 차단기를 차단하는데 필요한 실패 비율
  - circuitBreaker.sleepWindowInMilliseconds : 회로 차단기 차단 후 히스트릭스가 서비스 호출 시도를대기하는 시간
  - metrics.rollingStats.timeInMilliseconds : 히스트릭스가 수집하고 모니터링할 통계 시간
  - metrics.rollingStats.numBuckets : 히스트릭스가 유지할 측정 지표의 버킷 수



#### 히스트릭스 격리 전략

- THREAD
  - 부모 스레드와 컨텍스트를 공유하지 않는 격리된 스레드 풀에서 수행
- SEMAPHORE
  - 새로운 스레드를 시작하지 않고 분산 호출을 관리하며 타임아웃이 발생하면 부모 스레드를 중단시킴
- 히스트릭 팀은 대부분의 명령에 대해 THREAD방식을 권장, THREAD 격리 방식은 히스트릭스 명령 스레드와 부모 스레드 사이에 격리 수준을 높이며, SEMAPHORE 격리 방식보다 무겁다. SEMAPHORE 격리 모델은 경량이며, 서비스에서 대용량을 처리하고 비동기 I/O 프로그래밍 모델을 적용할 때 사용해야 한다.