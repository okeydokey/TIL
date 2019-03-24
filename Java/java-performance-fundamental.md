# [Java Performance Fundamental](<http://kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788995765371>)



- The Java Programming Language
  - Dynamic Linking
    - Class 파일은 JVM이 읽을 수 있는 형태로 번역된 것이고 JVM 위에서 다시 실행 가능한 형태로 변형되는데 이때 실행을 위한 Linking 작업이 일어난다. Class 파일은 실행시 Link를 할 수 있도록 Symbolic Reference 만을 가지고 있다. Symbolic Reference는 Runtime 시점에서 메모리상에서 실제로 존재하는 물리적인 주소로 대체되는 작업인 Linking 이 일어나게 된다. 이 Link 작업은 필요할 때마다 동적으로 이루어지기 때문에 Dynamic Linking 이라고 한다. Dynamic Linking 기술 덕에 Class 파일의 크기를 작게 유지할 수 있다.

- The Java Class File Format

  - Bytecode
    - Class 파일은 Bytecode를 binary 형태로 담아 놓은 것
    - Bytecode는 JVM이 읽을 수 있는 언어를 의미

  - Network Byte Order
    - Byte Order : 메모리의 주소 값을 할당하는 방식(Little Endian, Big Endian)
    - 서로 다른 계열의 CPU끼리 데이터를 전송 받을 때의 문제점을 해결하기 위해 정해진 일종의 약속
    - Big Endian 사용

- The Java Application Interface

  - JRE : 자바 런티임 환경으로 JVM, Java API, Native Method 등이 포함 됨

- The Java Virtual Machine(JVM)

  - 정의된 Specification을 구현한 하나의 독자적인 Runtime Instance로 하나의 프로세스 형태로 구동
  - JVM은 Class Loader System을 통해 Class 파일들을 JVM으로 로딩
  - 로딩된 Class 파일들은 Excution Engine을 통해 해석
  - 해석된 프로그램은 Runtime Data Areas에 배치되어 실질적인 수행
  - 이 실행 과정 속에서 JVM은 필요에 따라 Thread Synchronization, Garbage Collection 같은 관리작업 수행