# [Test Double](https://en.wikipedia.org/wiki/Test_double)
### Dummy object
- 단순 껍데기. 단지 인스턴스화된 객체가 필요 할 뿐 기능 까지 필요하지 않는 경우.

### Test stub
- 더미 객체가 마치 실제로 동작하는 것처럼 보이게 만들어 놓은 객체. 객체의 특정 상태를 가정.

### Test spy
- 메소드 정상 호출 여부 확인을 목적으로 구현.

### Mock Object
- 행위 기반 테스트.

### Fake Object
- 여러개의 인서턴스를 대표할 수 있는 경우이거나, 좀 더 복잡한 구현이 들어가 있는 객체.

## 상태 기반 테스트 vs 행위 기반 테스트
### 상태 기반 테스트
- 특정한 메소드를 거친 후, 객체의 상태에 대해 예상값과 비교하는 방식

### 행위 기반 테스트
- 특정한 동작 수행 여부 확인

#### 참조
- [테스트 주도 개발 : 고품질 쾌속개발을 위한 TDD 실천법과 도구](http://www.hanbit.co.kr/store/books/look.php?p_code=B3818551654)