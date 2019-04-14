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

  - 특별한 구성 없이 서비스 ID 기반으로 자동 라우팅

  - 유레카와 함께 주울을 사용하면 호출할 수 있는 단일 엔드포인트를 제공하고 주울 수정 없이 인스턴스 추가 및 제거

  - http://localhost:5555/**serviceId**/~ -> 서비스 ID를 이용해 서비스의 물리적 위치를 확인

  - /actuator/routes 를 이용해 엔드포인트 확인

    ```json
    {
        "/organizationservice/**": "organizationservice",
        "/configserver/**": "configserver",
        "/licensingservice/**": "licensingservice",
        "/specialroutesservice/**": "specialroutesservice"
    }
    ```

    

- 서비스 디스커버리를 이용한 수동 경로 매핑

  - ignored-service를 이용해 자동 생성된 유레카 서비스 ID 경로를 제외하고 사용자 정의한 경로만 사용

    ```yml
    zuul:
      ignore-service: 'organizationservice'
      routes:
      	organizationservice: /organization/**
    ```

  - /actuator/routes 를 이용해 엔드포인트 확인

    ```json
    {
        "/organization/**": "organizationservice",
        "/configserver/**": "configserver",
        "/licensingservice/**": "licensingservice",
        "/specialroutesservice/**": "specialroutesservice"
    }
    ```

  - prefix 프로퍼티를사용해 모든 서비스 경로 앞에 prefix를 추가

    ```yaml
    zuul:
      prefix: /api
    ```

  - /actuator/routes 를 이용해 엔드포인트 확인

    ```json
    {
        "/api/organization/**": "organizationservice",
        "/api/configserver/**": "configserver",
        "/api/licensingservice/**": "licensingservice",
        "/api/specialroutesservice/**": "specialroutesservice"
    }
    ```

- 정적 URL을 이용한 수동 경로 매핑

  - 유레카로 관리하지 않는 서비스를 주울을 사용해 고정 URL에 직접 라우팅

    ```yaml
    zuul:
      routes:
        licensestatic:
          path: /licensestatic/**
          url: http://licenseservice-static:8081
    ```

  - /actuator/routes 를 이용해 엔드포인트 확인

    ```json
    {
        "/api/licensestatic/**": "http://licenseservice-static:8081"
    }
    ```

  - 리본을 이용한 부하 분산 추가

    ```yaml
    zuul:
      routes:
        licensestatic:
          path: /licensestatic/**
          serviceId: licensestatic
    ribbon:
      eureka:
        enabled: false
    licensestatic:
      ribbon:
        listOfServers: http://licenseservice-static1:8081, http://licenseservice-static2:8082
    
    ```

  - /actuator/routes 를 이용해 엔드포인트 확인

    ```json
    {
        "/api/licensestatic/**": "licensestatic"
    }
    ```

    

- 경로 구성을 동적으로 로딩

  - POST /refresh

- 주울과 서비스 타임아웃 (spring-cloud-netflix-xxx-2.0.0.RELEASE.jar 으로 테스트)

  - 히스트릭스는 1초 이상 걸리는 모든 호출을 종료하고 HTTP 500 에러를 반환

    ```verilog
    2019-04-13 17:19:00.991 DEBUG 5505 --- [nio-5555-exec-6] c.t.zuulsvr.filters.TrackingFilter       : Processing incoming request for /api/organization/v1/organizations/timeout.
    2019-04-13 17:19:02.003  WARN 5505 --- [nio-5555-exec-6] o.s.c.n.z.filters.post.SendErrorFilter   : Error during filtering
      Caused by: java.net.SocketTimeoutException: Read timed out
    ```

    - hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds 히스트릭스 타임아웃 재정의
    - hystrix.command.**servicename**.execution.isolation.thread.timeoutInMilliseconds 서비스에 대해 별도 설정

  - ~~리본은 5초 이상 수행되는 호출을 타임아웃~~

    - 책에는 리본 타임아웃 설정이 5초라고 되어있으나 spring-cloud-netflix-ribbon-2.0.0.RELEASE.jar 기준으로 default 1초로 설정되어있어 히스트릭스 설정만 1초 이상으로 할 경우 1초에 끊김!!

    ```java
    public abstract class AbstractLoadBalancingClient<S extends ContextAwareRequest, T extends IResponse, D> extends AbstractLoadBalancerAwareClient<S, T> implements ServiceInstanceChooser {
    ...
      public void initWithNiwsConfig(IClientConfig clientConfig) {
              super.initWithNiwsConfig(clientConfig);
              RibbonProperties ribbon = RibbonProperties.from(clientConfig);
              this.connectTimeout = ribbon.connectTimeout(1000);
              this.readTimeout = ribbon.readTimeout(1000);
              this.secure = ribbon.isSecure();
              this.followRedirects = ribbon.isFollowRedirects();
              this.okToRetryOnAllOperations = ribbon.isOkToRetryOnAllOperations();
      }
    ...
    }
    ```

  - **servicename**.ribbon.ReadTimeout 리본 타임아웃 재정의

  - 히스트릭스 2초, 리본 3초 타임아웃 설정 할 경우 **리본 타임아웃 적용됨** (책과 다름), But 히스트릭스 타임아웃 오류 로그 발생

    - 하단 로직으로 2초 설정되지만, LB할때 리본 타임아웃이 적용 됨

    ```java
    public abstract class AbstractRibbonCommand<LBC extends AbstractLoadBalancerAwareClient<RQ, RS>, RQ extends ClientRequest, RS extends HttpResponse> extends HystrixCommand<ClientHttpResponse> implements RibbonCommand {
      ...
    		protected static int getHystrixTimeout(IClientConfig config, String commandKey) {
            int ribbonTimeout = getRibbonTimeout(config, commandKey);
            DynamicPropertyFactory dynamicPropertyFactory = DynamicPropertyFactory.getInstance();
            int defaultHystrixTimeout = dynamicPropertyFactory.getIntProperty("hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds", 0).get();
            int commandHystrixTimeout = dynamicPropertyFactory.getIntProperty("hystrix.command." + commandKey + ".execution.isolation.thread.timeoutInMilliseconds", 0).get();
            int hystrixTimeout;
            if (commandHystrixTimeout > 0) {
                hystrixTimeout = commandHystrixTimeout;
            } else if (defaultHystrixTimeout > 0) {
                hystrixTimeout = defaultHystrixTimeout;
            } else {
                hystrixTimeout = ribbonTimeout;
            }
    
            if (hystrixTimeout < ribbonTimeout) {
                LOGGER.warn("The Hystrix timeout of " + hystrixTimeout + "ms for the command " + commandKey + " is set lower than the combination of the Ribbon read and connect timeout, " + ribbonTimeout + "ms.");
            }
    
            return hystrixTimeout;
        }
      ...
    }
    ```

##### 주울 필터

- 게이트웨이를 통과하는 모든 서비스 호출에 대해 사용자 정의 로직을 작성
- 주울 3가지 필터 타입
  - 사전 필터
    - 실제 요청이 발생하기 전에 호출
    - 메시지 형식을 확인하는 작업을 수행, 인증, 인가 등 게이트키퍼 역할
  - 경로 필터
    - 대상 서비스를 호출하기 전에 호출을 가로채는 데 사용, 동적 라우팅 필요 여부를 결정
    - A/B 테스트
  - 사후 필터
    - 대상 서비스를 호출하고 응답을 클라이언트로 전송한 후 호출
    - 응답 로깅, 에러 처리, 민감한 정보에 대한 응답을 감시
- 구현
  - ZuulFilter 클래스 확장
    - filterType()를 Override 하여 필터 타입을 지정 
      - 'pre', 'post', 'route'
    - filterOrder()를 Override 하여 필터 우선순위 지정 
    - shouldFilter()를 Override 하여 필터 활성화 여부 지정
    - run()를 Override 하여 필터 통가시 실행될 로직 구현
  - com.netflix.zuul.context.RequestContext 사용



##### 주울 시작하기

- 의존성 추가

  ``` xml
  <dependency>
  	<groupId>org.springframework.cloud</groupId>
  	<artifactId>spring-cloud-starter-netflix-zuul</artifactId>
  </dependency>
  ```

- @EnableZuulProxy 애너테이션 추가



##### 주울에서의 유레카 사용

- 주울은 자동으로 유레카를 사용해 서비스 ID로 서비스를 찾은 후 넷플릭스 리본으로 주울 내부에서 요청에 대한 클라이언트 측 부하 분산을 수행

- 주울에서 리본을 사용할 경우 주울은 RibbonRoutingFilter 를 통해 라우팅

- 참고 : <https://stackoverflow.com/questions/43538030/zuul-and-ribbon-integration>

  

###### 상관관계 ID 란 ?

- 마이크로 서비스에서 다수 서비스 간 호출 체인을 추적하기 위해 사용
- 첫 호출  발생 시 전역 호출 식별자를 생성하고 후속하는 모든 호출에 전달하여 구조화된 방식으로 로그에 출력



##### 참고

- docker-compose -f ./docker/common/docker-compose-db.yml up