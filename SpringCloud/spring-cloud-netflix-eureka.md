# [Spring Cloud Netflix Eureka](https://cloud.spring.io/spring-cloud-netflix/spring-cloud-netflix.html#_service_discovery_eureka_clients)



### 용어 정리

- Servie Discovery
  - 분산된 자원의 물리적 위치 찾기
- eureka.client.serviceUrl.defaultZone



### Service Discovery

#### 로드 밸런서가 클라우드 기반에서 좋지 못한 이유?!

- 단일 장애 지점
  - 애플리케이션 인프라스트럭처 안에서 집중화된 병목 지점 ?
- 수평 확장의 제약성
  - 하드웨어 제약
  - 한정된 라이센스
- 정적 관리
- 복잡성

#### 클라우드에서 서비스 디스커버리

- 고가용성
- 피어 투 피어
- 부하 분산
- 회복성
- 장애 내성

#### 서비스 디스커버리 아키텍처

- 서비스 등록
- 클라이언트 서비스 주소 검색
- 정보 공유
- 상태 모니터링

#### 설정

- 