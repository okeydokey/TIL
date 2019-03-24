# [Run-Time Data Areas](https://docs.oracle.com/javase/specs/jvms/se10/html/jvms-2.html)

- Thread 별로 생성
  - PC Registers, Java Virtual Machine Stacks, Native Method Stacks
- 모든 Thread 공유
  - Method Area, Heap



### PC Registers

- JVM은 CPU에 직접 Instruction을 수행하지 않고 Stack에서 Operand를 뽑아내어 이를 별도의 메모리 공간인 PC Registers에 저장하는 방식

### Java Virtual Machine Stacks

- Thread의 수행 정보를 기록하는 Frame을 저장하는 메모리 영역

#### Stack Frame 

- Thread가 수행하고 있는 Application을 Method 단위로 기록하는 곳
- Local Variable Section, Operand Stack, Frame Data 로 구성
- 크기는 compile time에 결정

##### Local Variable Section

- Method의 Parameter Variable과 Local Variable 들을 저장
- 0부터 시작하는 인덱스를 가진 Array로 구성, 인덱스를 통해 데이터 접근
- Parameter Variable이나 Local Variable이 원시타입인 경우 고정된 크기로 할당, 객체일 경우 Heap의 위치를 말해주는 Reference 크기로 할당
- char, byte, short, boolean 형으로 선언할 경우 int형으로 할당, boolean을 제외하고 모든 원시타입은 JVM이 직접적으로 지원(?). char, byte, short는 Local Variable Section이나 Operand Stack에서는 int 형으로 변환하여 저장되고 Heap 등의 다른 곳에서는 원래의 형으로 원복하여 저장. boolean는그저 하나의 숫자에 불과하며  JVM 내부에서 원복되지 않고 int형 유지
- 0번째 인덱스
  - Local Method, Instance Method 에 무조건 포함되며 Heap 에 있는 Class 의 Instance 를 찾아가기 위한 정보

##### Operand Stack

- JVM 작업 공간으로 프로그램을 수행하면서 연산을 위해 사용되는 데이터 및 그 결과를 저장

##### Frame Data

- Constant Pool Resolution 정보, Method 정상종료 정보, 비정상 종료 시 Exception 관련 정보 저장
  - Resolution : Class File Format의 Symbolic Reference가 JVM에서 실제로 접근 할 수 있는 실질적인 Direct Reference로 변경되는 과정

### Native Method Stacks

- Java 외의 언어로 작성된 프로그램, API 툴킷 등과의 통합을 쉽게하기 위한 JNI라는 표준 규약을제공하며 Native Method를 호출하게 되면 Native Method Stacks에 새로운 Stack Frame을 생성하여 Native Fundtion을 계속 수행. Native Method의 수행이 끝나면 새로운 Stack Frame을 하나 생성하여 여기서 다시 작업을 수행

### Method Area

- Load 된 Type 정보를 저장
- JVM 기동할 때 생성되며 GC의 대상

##### Type Information

- Package.class 형태를 지니는 Type의 전체 이름
- Type의 직계 super class의 전체 이름
- class, interface 여부
- public, abstract, final 등 
- interface의경우 직접 링크(?) 되고 있는 객체의 리스트로 객체는 전체 이름 ?

##### Constant Pool

- Type의 모든 Constant(Literal Constant, Type, Field, Method) 정보
- JVM은 실행 시 참조하는 객체에 접근 할 필요가 있으면 Constant Pool의 Symbolic Reference를 통해 해당 객체가 위치한 메모리의 주소를 찾아 동적으로 연결

##### Field Information

- Field 이름
- Field의 Data Type, 선언 순서
- public, private, protected, static, final, volatile, transient 등

##### Method Information

- Method 이름
- Method 반환 값의 DataType 또는  void
- Method Parameter 수 와 DataType, 선언된 순서
- public, private, protected, static, final, synchronized, native, abstract 등
- (native나 abstract가 아니라면) Method의 Bytecode, Method Stack Frame의 Operand Stack, Local Variable Section의 크기, Exception Table

##### Class Variables

- Class에서 static으로 선언된 변수로 Class 에서 하나의 값으로 유지 됨
- Class를 사용하기 전부터 Method Area에 미리 메모리를 할당받음
- final로 선언될 경우 Constant Pool의 Literal Constant로 저장 됨

##### Reference to class ClassLoader

##### Reference to class Class

##### Method Table

- Class의 Method에 대한 Direct Reference를 갖는 자료 구조

### Heap

- Instance 와 Array 객체 두 가지 종류 저장 공간

#### Object Layout

- Header와 Data로 나뉘어짐

##### Hotspot JVM의 Object Layout

- Header 
  - Mark Word : GC와 Synchronization을 위해 사용
  - Class Address : Method Area의 Class 정보를 가리키는 Reference 정보
  - Array Size : Array만 가지는 Header

#### Heap 구조

##### Hotspot JVM의 Heap 구조

- Young Generation, Old Generation으로 나누어져 있는 Generational Heap
- Young Generation
  - Eden 영역과 Survivor 영역으로 구성됨
  - Eden 영역은 Object가 Heap에 처음으로 할당되는 장소이며 Eden Area가 꽉 차게되면 Object의 참조 여부를 따져 만약 참조가 되어 있는 Live Object면 Survivor영역으로 넘기고, 참조가 끊어진 Garbage Object 이면 그냥 남겨 놓음, 모든 Live Object가 Survivor 여역으로 넘어가면 Eden 영역을 모두 청소
- Old Generation
  - Young Generation에서 Live Object로 오래 살아남아 성숙된 Object는 Old Generation으로 이동, 이를 Promotion이라고 함

