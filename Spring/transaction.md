# Transaction



- Multiple Datasource를 사용할 경우 @Trasactional을 사용할때 transactionManager를 지정해 줘야한다. 지정해 주지 않으면 기본값이 설정되어 특정 Datasource 사용 시 Transaction이 제대로 동작하지 않을 수 있다.

  - @Trasactional("xxxTransactionManager")

  - Annotation을 만들어 사용할 수 있음

  - ```java
    @Target({ElementType.METHOD, ElementType.TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @Transactional("xxxTransactionManager")
    public @interface xxxTx {
    }
    ```

- @Transactional 과 메소드 가시성

  > When using proxies, you should apply the `@Transactional` annotation only to methods with *public* visibility

  - Proxy(기본값) 를 사용할때 @Transactional 을 사용하는 메소드는 반드시! public 이여야 한다. public이 아니면 무시된다.

- Read Only

  - 읽기만 할 경우에 왜 트랜젝션 설정이 필요한가 ? 쓰기를 방지해 주는걸 보장하진 못한다. 그러면 왜 ?
    - 최적화 해준다고 한다... 더 알아봐야할듯!
