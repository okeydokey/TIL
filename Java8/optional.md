# Optional

참고 : [자바 8 인 액션](http://www.hanbit.co.kr/store/books/look.php?p_code=B1999551123)



### null 때문에 발생하는 문제

- 에러의 근원 : NullPointerException

- 중첩된 null 확인 코드로 인한 가독성 문제

- 아무 의미 없는 null

- 개발자로부터 포인터를 숨기고자 한 자바의 철학 위배, null 포인터



### java.util.Optional<T>

#### Optional 객체 만들기

- 빈 Optional
  - Optional.empty()
- null이 아닌 값으로 Optional 만들기
  - Optional.of(value)
  - value가 null이라면 즉시 NullPointerException이 발생
- Null값으로 Optional 만들기
  - Optional.ofNullable(value)
  - value가 null이면 빈 Optional 객체를 반환

#### map으로 Optional의 값을 추출하고 변환하기

- Optional.ofNullable(value).map(value::getCar).map(car::getName)
  - getCar의 반환값이 Optional<Car> 일 경우 .map(value::getCar) 의 반환값은 Optional<Optional<Car>> 으로 컴파일 오류 발생
- Optional이 중첩일 경우 flatMap 이용
  - Optional.ofNullable(value).flatMap(value::getCar).map(car::getName)
- 호출 체인 중 어떤 메서드가 빈 Optional을 반환한다면 전체 결과로 빈 Optional을 반환

> Optional 클래스 설계자는 Optional의 용도가 반환값을 지원하는 것이라고 명확하게 못박았다. 도메인 모델에서 필드에서 사용 하지 말것. 도메인 모델 필드에 Optional을 사용한다면 직렬화에 문제가 생길 수 있다.

``` 
public class Person {
	// private Optional<Car> car; // 직렬화가 필요하다면 이렇게 사용하지 말것
	private Car car;
	public Optional<Car> getCarAsOptional() {	// Optional로 값을 반환 받을 수 있는 메서드를 추가하는 방식을 권장
		return Optional.ofNullable(car);
	}
}
```

#### 디폴트 액션과 Optional 언랩

- get()
  - 가장 간단하면서 가장 안전하지 않은 메서드, 값이 없을 경우 NoSuchElementException 반환
  - 값이 반드시 있다고 가정할 수 있는 상황이 아니면 get 메서드는 **사용하지 말것**
- orElse(T other)
  - 값을 포함하지 않을때 디폴트값을 제공할 수 있음
- orElseGet(Supplier<? extends T> other)
  - orElse(T other) 메서드의 게으름 버전, 값을 포함하지 않을때만 Supplier 실행
- orElseThrow(Supplier<? extends T> exceptionSupplier)
  - Optional이 비어있을 때 예외를 발생 시킬 수 있음
- ifPresent(Consumer<? super T> consumer)
  - 값이 존재할 때 인수로 넘겨준 동작을 실행, 값이없으면 실행되지 않음



#### 두 Optional 합치기

- null 확인 코드와 별 다르지 않은 좋지 않은 방법

  ```java
  public Insurance findCheapestInsurance(Person person, Car car) {
  	// 다양한 보험회사가 제공하는 서비스 조회
  	// 모든 결과 데이터 비교
  	return cheapestCompany;
  }
  
  public Optional<Insurance> nullSafeFindCheapestInsurance(Optional<Person> person, Optional<Car> car) {
    if(person.isPresent() && car.isPresent()) {
    	return Optional.of(indCheapestInsurance(person.get(), car.get()));
    } else {
    	return Optional.empty();
    }
  }
  ```

- Optional 업랩하지 않고 합치기

- ```java
  public Optional<Insurance> nullSafeFindCheapestInsurance(Optional<Person> person, Optional<Car> car) {
  	return person.flatMap(p -> car.map(c -> findCheapestInsurance(p, c)));
  }
  ```

  

#### 필터로 특정값 거르기

```
Insurance insurance = ...;
if(insurance != null &&  "CambridgeInsurance".equals(insurance.getName())) {
  System.out.println("ok");
}

Optional<Insurance> optInsurance = ...;
optInsurance.filter(insurance -> "CambridgeInsurance".equals(insurance.getName()))
						.ifPresent(x -> System.out.println("ok"));
```

- filter 메서드는 프레디케이트를 인수로 받으며 Optional 객체가 값을 가지고 프레디케이트와 일치하면 그 값을 반환, 아닐 경우 빈 Optional 객체를 반환



#### Optional을 사용한 실용 예제

- 잠재적으로 null이 될 수 있는 대상을 Optional로 감싸기
- 예외와 Optional

> 기본형 특화 Optional(OptionalInt, OptionalLong, OptionalDouble)는 map, flatMap, filter 등을 지원하지 않고 일반 Optional과 혼용할 수 없으므로 사용을 권장하지 않음



