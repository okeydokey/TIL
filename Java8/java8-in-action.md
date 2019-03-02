# Java8 인 액션



### map vs flatMap



### Arrays.stream(int []) vs List<Integer>.stream()

- Arrays.stream() -> IntStream, List<Integer>.stream() -> Stream<Integer>

- https://stackoverflow.com/questions/29028980/why-cant-i-map-integers-to-strings-when-streaming-from-an-array



### 기본형 특화 스트림

- IntStream, DoubleStream, LongStream
- IntStream -> Stream<Integer>
  - boxed()
- Stream<Integer> -> IntStream
  - mapToInt

- 요소가 없는 상황과 실제 값인 상황을 구별
  - OptionalInt
- range, rangeClosed
  - range는 시작값과 종료값을 결과에 포함하지 않음 ex) range(1,100) 일 경우 1과 100은 포함되지 않음 2~99 