# 2장 객체 생성과 파괴

## item1. 생성자 대신 정적 팩터리 메서드를 고려하라
- 클래스는 클라이언트에 public 생성자 대신 (혹은 생성자와 함께) 정적 팩터리 메서드를 제공할 수 있다.
- 정적 팩터리 메서드 : 그 클래스의 인스턴스를 반환하는 단순한 정적 메서드

### 장점
- 이름을 가질 수 있다.
    - 반환될 객체의 특성을 메소드명에 드러낼 수 있다.
- 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.
    - 인스턴스를 통제 할 수 있다. ex) 싱글턴, 인스턴스화 불가, 불변 클래스 재활용, 인스턴스 캐싱 등
- 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
    - 유연성 제공 ex) java.util.Collections
        ```java 
        public static <T> List<T> unmodifiableList(List<? extends T> list) {
            return (list instanceof RandomAccess ?
                    new UnmodifiableRandomAccessList<>(list) :
                    new UnmodifiableList<>(list));
        }
        ```
- 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
    - ex) java.util.EnumSet
        ```java
        public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
            Enum<?>[] universe = getUniverse(elementType);
            if (universe == null)
                throw new ClassCastException(elementType + " not an enum");

            if (universe.length <= 64)
                return new RegularEnumSet<>(elementType, universe);
            else
                return new JumboEnumSet<>(elementType, universe);
        }
        ```
- 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.
    - 인터페이스를 반환 타입으로 사용하므로 구현체가 존재하지 않더라도 작성 가능하며 클라이언트를 구현체로부터 분리할 수 있다.
    - ex) JDBC
        ```java
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/test", "test","1234");
        ```    

### 단점
- 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
- 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.
    - 규약에 따라 메서드 명을 짓는 식으로 완화해줘야 한다.
        - from : 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드
            ```java 
            Date d = Date.from(instant);
            ```
        - of : 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드
            ```java 
            Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING)
            ```
        - valueOf : from과 of의 더 자세한 버전
            ```java 
            BigIntger prime = BigIntger.valueOf(Integer.MAX_VALUE);
            ```
        - instance 혹은 getInstance : 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다.
            ```java 
            StackWalker luke = StackWalker.getInstance(options);
            ```
        - create 혹은 newInstance : instance 혹은 getInstance와 같지만, 매법 새로운 인스턴스를 생성해 반환함을 보장한다.
            ```java 
            Object newArray = Array.newInstance(classObject, arrayLen);
            ```
        - getType : getInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. "Type"은 팩터리 메서드가 반환할 객체의 타입이다.
            ```java 
            FileStore fs = Files.getFileStore(path);
            ```
        - newType : newInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. "Type"은 팩터리 메서드가 반환할 객체의 타입이다.
            ```java 
            BufferedReader br = Files.newBufferedReader(path);
            ```
        - type : getType과 newType의 간결한 버전
            ```java 
            List<Complaint> litany = Collections.list(legacyLitany);
            ```
### 배경 지식
- 플라이웨이트 패턴
- 동반 클래스(companion class)
- 서비스 제공자 프레임워크(service provider famework)
    - 서비스 인터페이스(service interface) : 구현체의 동작을 정의
    - 제공자 등록 인터페이스 API(provider registration API) : 제공자가 구현체를 등록할 때 사용
    - 서비스 접근 API(service access API) : 클라이언트가 서비스의 인스턴스를 얻을 때 사용
    - 서비스 제공자 인터페이스(service provider interface, 선택) : 서비스 인터페이스의 인스턴스를 생성하는 팩터리 객체 설명?
    - ex) JDBC
        - 서비스 인터페이스 : Connection
        - 제공자 등록 API : DriverManager.registerDriver
        - 서비스 접근 API : DriverManager.getConnection
        - 서비스 제공자 인터페이스 : Driver
- 브리지 패턴
- java.util.ServiceLoader

## item2. 생성자에 매개변수가 많다면 빌더를 고려하라
- 선택적 매개변수가 많다면 점층적 생성자 패턴도 쓸 수 있지만, 매개변수 개수가 많아지면 클라이언트 코드를 작성하거나 읽기 어렵다.
- 다른 대안으로 사용하는 자바빈즈 패턴에서는 객체 하나를 만들려면 메서드를 여러개 호출해야 하고, 객체가 완전히 생성되기 전까지는 일관성이 무너진 상태가 된다. 이때문에 클래스를 불변으로 만들 수 없으며 스레드 안전성을 얻기위해서 추가 작업이 필요하다.
- 빌더 패턴은 점층적 생성자 패턴의 안전성과 자바빈즈 패턴의 가독성을 겸비한 대안이다.
    ```java
    public class NutritionFacts {
        private final int servingSize;
        private final int servings;
        private final int calories;
        private final int fat;
        private final int sodium;
        private final int carbohydrate;

        public static class Builder {
            // Required parameters(필수 인자)
            private final int servingSize;
            private final int servings;

            // Optional parameters - initialized to default values(선택적 인자는 기본값으로 초기화)
            private int calories      = 0;
            private int fat           = 0;
            private int carbohydrate  = 0;
            private int sodium        = 0;

            public Builder(int servingSize, int servings) {
                this.servingSize = servingSize;
                this.servings    = servings;
            }

            public Builder calories(int val) {
                calories = val;
                return this;    // 이렇게 하면 . 으로 체인을 이어갈 수 있다.
            }
            public Builder fat(int val) {
                fat = val;
                return this;
            }
            public Builder carbohydrate(int val) {
                carbohydrate = val;
                return this;
            }
            public Builder sodium(int val) {
                sodium = val;
                return this;
            }
            public NutritionFacts build() {
                return new NutritionFacts(this);
            }
        }

        private NutritionFacts(Builder builder) {
            servingSize  = builder.servingSize;
            servings     = builder.servings;
            calories     = builder.calories;
            fat          = builder.fat;
            sodium       = builder.sodium;
            carbohydrate = builder.carbohydrate;
        }
    }
    ```
    ```java
    NutritionFacts cocaCola = new NutritionFacts
    .Builder(240, 8)    // 필수값 입력
    .calories(100)
    .sodium(35)
    .carbohydrate(27)
    .build();           // build() 가 객체를 생성해 돌려준다.
    ```

### 의문 ?
- 빌더 하나로 여러 객체를 순회하면서 만든다는 의미가 무엇인가 ?

### 배경 지식
- freezing 메서드
- 공변 반환 타이핑

## item3. private 생성자나 열거 타입으로 싱글턴임을 보증하라
- 싱글턴이란 인스턴스를 오직 하나만 생성할 수 있는 클래스를 말한다.
- public static final 필드 방식의 싱글턴과 정적 팩터리 방식의 싱글턴은 직렬화 할때 추가 작업이 필요하다. (transient선언, readResolve메서드 제공) 이렇게 하지 않으면 직렬화된 인스턴스를 역질렬화할 때마다 새로운 인스턴스가 만들어 진다.
- 싱글턴을 만드는 세 번째 방법은 원소가 하나인 열거 타입을 선언하는 것이다. 추가 노력 없이 직렬화할 수 있으며 리플렉션 공격에서도 제2의 인스턴스가 생기는 일을 완벽하게 막아준다. 대부분 상황에서 원소가 하나뿐인 열거타입이 싱글턴을 만든는 가장 좋은 방법이지만 상속이 필요할 경우 이 방법은 사용할 수 없다.

## item4. 인스턴스화를 막으려거든 private 생성자를 사용하라

## item5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라
- 클래스가 내부적으로 하나 이상의 자원에 의존하고, 그 자원이 클래스 동작에 영향을 준다면 싱글턴과 정적 유틸리티 클래스는 사용하지 않는 것이 좋다. 이 자원들을 클래스가 직접 만들게 해서도 안된다. 대신 필요한 자원을 생성자에 넘겨주자.
- 생성자에 자원 팩터리(호출할 때마다 특정 타입의 인스턴스를 반복해서 만들어주는 객체)를 넘겨주는 방식을 사용할 수도 있다. 자바8에서 Supplier\<T> 인터페이스를 사용해 팩터리를 넘길수 있다.

### 배경 지식
- Supplier\<T>
    ```java
    @FunctionalInterface
    public interface Supplier<T> {

        /**
        * Gets a result.
        *
        * @return a result
        */
        T get();
    }
    ```

## item6. 불필요한 객체 생성을 피하라
- 같은 기능의 객체를 매번 생성하기보다는 객체 하나를 재사용하는 편이 나을때가 많다. 특히 불변 객체는 언제든 재사용할 수 있다.
- String s = new Stirng("bikini"); 이 코드는 호출 시 마다 쓸떼없는 String 인스턴스를 만든다. 
- String s = "bikini"; 이 코드는 하나의 String 인스턴스를 사용한다. 이방식을 사용한다면 같은 가상 머신 안에서 이와 똑같은 문자열 리터럴을 사용하는 모든 코드가 같은 객체를 재사용함이 보장된다.
- 생성자 대신 정적 팩터리 메서드를 제공하는 불변 클래스에서는 정적 팩터리 메서드를 사용해 불필요한 객체 생성을 피할 수 있다. 예컨데 Boolean(String) 생성자 대신 Boolean.valueOf(String) 팩터리 메서드를 사용하는 것이 좋다.
- 생성 비용이 비싼 객체가 반복해서 필요하다면 캐싱하여 재사용하길 권한다.
- 박싱된 기본 타입보다는 기본 타입을 사용하고, 의도치 않은 오토박싱이 숨어들지 않도록 주의하자

## item7. 다 쓴 객체 참조를 해제하라
- 예
    ```java
    // 메모리 누수 발생
    public Object pop() {
        if(size ==0)
            throw new EmptyStackException();
        return elements[--size];
    }
    // 다 쓴 객체 참조 해제
    public Object pop() {
        if(size ==0)
            throw new EmptyStackException();
        Object result = elements[--size];
        elements[size] = null;
        return result;

    }

    ```
- 객체 참조를 null 처리하는 일은 예외적인 경우여야한다. 다 쓴 참조를 해제하는 가장 좋은 방법은 그 참조를 담은 변수의 범위를 최소가 되게 정의해서 유효 범위 밖으로 밀어내는 것이다.
- 캐시 역시 메모리 누수를 일으키는 주범이다. 캐시 외부에서 키를 참조하는 동안만 엔트리가 살아 있는 캐시가 필요한 사항이라면 WeakHashMap을 사용해 캐시를 만들자.
- 리스너 혹은 콜백도 메모리 누수의 주범이다. 클라이언트가 콜백을 등록만 하고 명확히 해지하지 않는다면 계속 쌓여갈 것이다.(?) 콜백을 약한 참조로 저장하면 가비지 컬렉터가 즉시 수거해간다.

## item8. finalizer와 cleaner 사용을 피하라
- finalizer는 예측할 수 없고, 상황에 따라 위험할 수 있어 일반적으로 불필요하다.
- cleaner는 finalizer보다는 덜 위험하지만, 여전히 예측할 수 없고, 느리고, 일반적으로 불필요하다.
- 종료해야할 자원을 담고 있는 객체의 클래스의 경우 AutoCloseable을 구현하고 close를 호출하자.

## item9. try-finally보다는 try-with-resources를 사용하라

