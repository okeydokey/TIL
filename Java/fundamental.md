# [자바의 정석](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&barcode=9788994492032)

## Chapter1. 자바를 시작하기 전에

### 자바(Java Programming Language)
- 1996년 1월 썬에서 발표한 객체지향 프로그래밍 언어
- 1991년 Oak(오크)라는 언어에서부터 시작하였으며 인터넷이 등장하자 가전제품 탑재 소프트웨어를 목적으로 하였던 것을 개발 방향을 바꾸면서 이름을 Java로 변경
- 일반 어플리케이션의 코드는 OS만 거치고 하드웨어로 전달되는데 비해 Java어플리케이션은  JVM을 한 번 더 거쳐 실행 시 해석(interpret)되기 때문에 속도가 느리다는 단점을 가지고 있으나 컴파일된 자바코드(바이트 코드)를 하드웨어의 기계어로 바로 변환해주는 JIT컴파일러와 향상된 최적화 기술이 적용되어 속도의 격차가 많이 줄어듦

### [Java API](https://docs.oracle.com/javase/8/docs/api/)

### 자바로 프로그램 작성하기
- 애플릿(applet)이나 서블릿(servlet)은 main메서드 대신 유사한 역할을 하는 다른 메서드가 존재
- 자바프로그램 실행과정
	1. 프로그램의 실행에 필요한 클래스(*.class)를 로드
	2. 클래스 파일을 검사 (파일형식, 악성코드 체크)
	3. 지정된 클래스에서 main(String[] args)를 호출

## Chapter2. 변수

### 변수
- 변수명은 숫자로 시작해서는 안되며 특수문자는 ‘_’와 ‘$’만을 허용

### 변수의 타입
- 기본형(boolean, char, byte, short, int, long, float, double) 8개, 참조형
- 참조형 변수는 null 또는 객체의 주소(4byte, 0x0~0xffffffff)를 값으로 갖음
- 기본형은 저장할 값의 종류에 따라 구분되므로 기본형 변수의 종류는 자료형(Data Type)이라는 용어를 쓰고 참조형 변수는 객체의 종류에 의해 구분되므로 타입(Type)이라는 용어를 사용
- 기본형

|  | 1byte | 2byte | 4byte | 8byte |
| ------- | ------- | -------- | ------- | ------- |
| 논리형 | boolean | | | |
| 문자형 | | char | | |
| 정수형 | byte | short | **int** | long |
| 실수형 | | |float | **double** |

- boolean은 true와 false, 2가지의 값만 표현하면 되므로 1bit만으로도 충분하지만 데이터를 다루는 최소 단위가 1byte이기 때문에 1byte를 사용
- Java에서는 대소문자를 구별하기 때문에 TRUE와 true는 다른 것으로 간주
- C언어와 같은 프로그래밍 언어는 문자형의 경우 1byte(확장된 ASCII코드)의 크기를 갖지만, Java에서는 유니코드(Unicode) 문자 체계를 사용하기 때문에 크기가 2byte 임
- char grade = ‘’; 는 에러 char grade = ‘ ‘;로 초기화. 기본값은 char grade= ‘\u0000’;
- 유니코드는 세계 각 국의 언어를 통일된 방법으로 표현할 수 있게 제안된 국제적 코드규약. 영문 알파벳은 1byte만으로 충분했지만 한글, 한자 또는 일어 등과 같은 문자는 1byte로 표현이 불가능하기에 2byte로 문자를 표현하는 유니코드가 만들어 짐
- ASCII코드는 128개의 문자집합을 제공하는 7bit 부호로, 처음 32개의 부호는 인쇄와 전송 제어용으로 사용. 보통은 ASCII코드를 확장해서 8bit로 된 문자집합을 사용하는데 128개의 ASCII코드와 특수문자들로 이루어 짐
- 정수형은 10진수 외에도 16진수 또는 8진수로 표현된 정수를 변수에 저장. 16진수 : 0x, 8진수 : 0을 붙임

## Chapter3. 연산자

### 단항 연산자
- i=i+1과 ++i의 바이트코드 비교. i=i+1에 비해 ++i가 훨씬 적은 명령만으로 수행됨. 또, 덧셈 연산자는 필요에 따라 피연산자를 형변환하지만 증감 연산자는 형변환 없이 피연산자의 값을 변경

|수식|i=i+1;|++i;|
|-----|-------|-----|
|컴파일된 코드|istore_1|istore_1|
||iload_1|iinc 1 1|
||iconst_1||
||iadd||
||istore_1||

- 연산자 ‘~’는 피연산자의 타입이 int형 보다 작으면 int형으로 변환한 다음 연산을 하기때문에 연산결과가 int형이 됨. 연산자 ‘~’의 연산결과를 저장하기 위해서는 int형 변수에 담거나 캐스트 연산자를 사용

### 산술 연산자
- 모든 이항 연산자는연산을 수행하기 전에 크기가 4byte이하인 자료형을 int형으로 변환, 피현산자들의 타입 일치.
- 상수 또는 리터럴 간의 연산은 실행과정동안 변하는 값이 아니기 때문에, 컴파일 시에 컴파일러가 계산해서 그 결과로 대체함으로써 보다 효율적
- 나머지 연산자 : %연산자의 왼쪽에 있는 피연산자의 부호를 따름
- 쉬프트 연산자 : 정수형 변수에만 사용, x << n은 x * 2<sup>n</sup>, x >> n은 x / 2<sup>n</sup>, 속도가 빠름.

### 비교 연산자
- 자료형의 범위가 큰 쪽으로 형변환하여 피연산자의 타입을 일치시킨 후 비교
- 참조형의 경우 객체의 주소값을 비교

### 논리 연산자
- 비트 연산자 \|와 & 역시 피연산자로 boolean형을 허용하므로 조건식간의 연결에 사용할 수 있지만 \|\|나 &&와는 달리 항상 양 쪽의 피연산자를 모두 검사

## Chapter4. 조건문과 반복문

### switch문
- Java7에서 switch Expression에 기존에 허용되지 않던 String 타입이 추가됨
- case문에는 오로지 리터럴이나 상수만을 허용, 변수는 허용되지 않음
- if문보다 더 간결하고 속도가 빠름

### 반복문
- for(int j=0; j < 1000000000; j++) ; 도 가능

## Chapter5. 배열

### 배열
- 배열 선언
	- 타입[] 변수이름; // int[] score; String[] name;
	- 타입 변수이름[]; // int score[]; String name[];
- 배열 생성
	- new 와 함게 배열의 타입과 크기를 지정
	- score = new int[5];
- 배열 초기화
	- int[] score = { 100, 90, 80, 70, 60 };
	- int[] score = new int[] { 100, 90, 80, 70, 60 };
	- String[] name = { new String(“Kim”), new String(“Park”) };
	- String[] name = { “Kim”, “Park” };
	- String[] name = new String[] { new String(“Kim”), new String(“Park”) };
	- 선언과 초기화를 따로해야하는 경우 반드시 score = new int[] { 100, 90, 80, 70, 60 }; 방식으로 배열 초기화
	- int 배열을 받는 메서드의 매개변수로 전달할 경우에도 score = new int[] { 100, 90, 80, 70, 60 }; 방식으로 배열 초기화
	- 배열 복사
		- System클래스의 arraycopy()를 사용
		- System.arraycopy(arr1, 0, arr2, 0, arr1.lenght); // arr1[0]에서 arr2[0]으로 arr1.length개의 데이터를 복사
		- arraycopy()는 배열에 저장되어 있는값만 복사하기 때문에 참조변수 배열인 경우에는 단지 주소값만을 복사할 뿐 참조변수가 가리키고 있는 객체를 복사하지 않음

### 다차원 배열
- 선언
	- 타입[][] 변수이름; // int[][] score;
	-  타입 변수이름[][]; // int score[][];
	- 타입[] 변수이름[]; // int[] score[];

### 가변 배열
- 2차원 이상의 다차원 배열을 생성할 때 전체 배열 차수 중 마지막 차수의 크기를 지정하지 않고 추후에 각기 다른 크기의 배열을 생성함으로써 고정된 형태가 아닌 보다 유동적인 가변 배열을 구성

## Chapter6. 객체지향 프로그래밍1

### 객체지향언어
- 과학자들은 모의실험을 위해 실제 세계와 유사한 가상 세계를 컴퓨터 속에 구현하고자 노력하여 객체지향 이론 탄생
- 실제 세계는 사물(객체)로 이루어져 있으며, 발생하는 모든 사건들은 사물간의 상호작용
- 상속, 캡슐화, 추상화 개념을 중심으로 점차 구체적으로 발전되었으며 1960년대 중반에 객체지향이론을 프로그래밍언어에 적용한 시뮬라(Simula)라는 최초의 객체지향언어가 탄생

### 변수와 메서드

|변수의 종류|선언위치|생성시기|
|-------------|----------|----------|
|클래스변수(static 멤버변수)|클래스영역|클래스가 메모리에 올라갈 때|
|인스턴스변수(멤버변수)|클래스영역|인스턴스가 생성되었을 때|
|지역변수|클래스 영역 이외의 영역(메서드, 생성자,  초기화 블럭 내부)|변수 선언문이 수행되었을 때|

- 클래스변수는 클래스가 로딩될 때 생성되어 프로그램이 종료될 때 까지 유지되어, public을 앞에 붙이면 같은 프로그램 내에서 어디서나 접근할 수 있는 전역변수(global variable)의 성격을 갖음
- 클래스변수를 사용할 때는 클래스명.클래스변수의 형태로 하는 것이 좋음. 인스턴스변수로 오해하기 쉬움

#### JVM의 메모리구조
- Method Area
	- 프로그램 실행 중 클래스가 사용되면 JVM은 해당 클래스의 클래스파일(*.class)을 읽어서 분석하여 클래스에 대한 정보(클래스 데이터)를 이곳에 저장. 클래스 변수도 이 영역에 생성
- Heap
	- 인스턴스가 생성되는 공간. 인스턴스 변수들이 생성되는 공간.
- Call Stack
	- 메서드의 작업에 필요한 메모리 공간을 제공
	- 메서드가 호출되면 호출 스택에 호출된 메서드를 위한 메모리가 할당되며 작업을 수행하는 동안 지역변수(매개변수 포함)들과 연산의 중간결과 등을 저장하는데 사용. 메서드가 작업을 마치면 메모리 공간 반환.

#### 클래스메서드(static)와 인스턴스메서드
1. 클래스를 설계할 때, 멤버변수 중 모든 인스턴스에 공통적으로 사용해야하는 것에 static 붙임
2. 클래스변수는 인스턴스를 생성하지 않아도 사용
3. 클래스메서드는 인스턴스변수 사용 불가
4. 메서드 내에서 인스턴스변수를 사용하지 않는다면, static 붙이는 것을 고려

- 클래스멤버가 인스턴스멤버를 참조 또는 호출하고자 하는 경우에는 인스턴스를 생성해야 함
- 인스턴스멤버가 존재하는 시점에 클래스멤버는 항상 존재하지만, 클래스멤버가 존재하는 시점에 인스턴스멤버가 존재할 수도 있고 존재하지 않을 수도 있기 때문

### 메서드 오버로딩
- 메서드 이름이 같아야 함
- 매개변수의 개수 또는 타입이 달라야 함
- 매개변수는 같고 리턴타입이 다른 경우는 오버로딩이 성립되지 않음

### 생성자
- 인스턴스 초기화 메서드
- Card c = new Card();
	- 연산자 new에 의해서 메모리(heap)에 Card클래스의 인스턴스가 생성 됨
	- 생성자 Card()가 호출되어 수행
	- 연산자 new의 결과로 생성된 Card인스턴스의 주소가 반환되어 참조변수 c에 저장 됨
- 기본 생성자가 컴파일러에 의해서 추가되는 경우는 클래스에 정의된 생성자가 하나도 없을 때 뿐
- 생성자에서 다른 생성자 호출
	- 생성자의 이름으로 클래스 이름 대신 this 사용
	- 한 생성자에서 다른 생성자 호출 시 반드시 첫줄에서만 호출 가능
		- 다른 생성자 호출하기 이전의 초기화 작업이 무의미해 줄 수 있기 때문

#### this
- this는 참조변수로 인스턴스 자신을 가리킴
- this를 사용 할 수 있는 것은 인스턴스 멤버 뿐. static메서드에서 인스턴스 멤버들을 사용할 수 없는 것처럼 this역시 사용할 수 없음. static메서드는 인스턴스를 생성하지 않고도 호출될 수 있으므로 static메서드가 호출된 시점에 인스턴스가 존재하지 않을 수도 있기 때문

### 변수의 초기화
- 멤버변수(클래스변수와 인스턴스변수)와 배열의 초기화는 선택적이지만, 지역변수는 반드시 사용하기 전 초기화 필요
- 멤버변수 초기화 방법
	- 명시적 초기화
	- 생성자
	- 초기화 블럭
		- 클래스 초기화 블럭 static {}
			- 클래스가 메모리에 처음 로딩될 때 한번만 수행
		- 인스턴스 초기화 블럭 {}
			- 생성자와 같이 인스턴스를 생성할 때 마다 수행
			- 생성자보다 인스턴스 초기화 블럭이 먼저 수행
			- 클래스의 모든 생성자에서 공통적으로 수행되어야 하는 코드가 있는 경우 인스턴스 초기화 블럭을 사용하여 중복 제거

#### 멤버변수의 초기화 시기와 순서
- 클래스변수 초기화 시점: 클래스가 처음 로딩될 때 단 한번 초기화
- 인스턴스변수 초기화 시점 : 인스턴스가 생성될 때마다 각 인스턴스별로 초기화
- 클래스변수 초기화 순서 : 기본값 -> 명시적초기화 -> 클래스 초기화 블럭
- 인스턴스변수의 초기화순서 : 기본값 -> 명시적초기화 -> 인스턴스 초기화 블럭 -> 생성자
- 클래스 변수는 항상 인스턴스 변수보다 항상 먼저 생성되고 초기화 됨

## Chapter7. 객체지향 프로그래밍2

### 상속
- 생성자와 초기화 블럭은 상속되지 않음

### 오버라이딩
- 오버라이딩 조건
	- 이름, 매개변수, 리턴타입이 같아야 함
	- 접근 제어자는 조상 클래스의 메서드보다 좁은 범위로 변경 할 수 없음
	- 조상 클래스의 메서드보다 많은 수의 예외를 선언 할 수 없음
	- 인스턴스메서드를 static 메서드로 또는 그 반대로 변경 할 수 없음

#### super()
- 생성자 첫 줄에 반드시 자신의 다른 생성자 또는 조상의 생성자를 호출, 그렇지 않으면 컴파일러가 super() 자동 추가. 조상 클래스에 디폴트 생성자가 없을 경우 컴파일 에러 발생. 적절한 생성자 추가 필요.
- 조상 클래스의 멤버변수는 조상의 생성자에 의해 초기화 되도록 할 것

### package와 import
- 컴파일시 -d 옵션을 주면 소스파일에 지정된 경로를 통해 패키지의 위치를 찾ㄷ아서 클래스 파일을 생성
- -cp옵션을 이용해서 일시적으로 클래스패스 지정 가능
- import문은 프로그램 성능에 전혀 영향을 미치지 않으며 컴파일 시간이 아주 조금 증가(패키지명.* 마찬가지)

### 제어자
- 접근 제어자
	- public, protected, default, private
- 그 외
	- static, final, abstract, native, transient, synchronized, volatile, strictfp

#### static
- 사용될 수 있는 곳
	- 멤버변수, 메서드, 초기화 블럭
- static 초기화 블럭은 클래스가 메모리에 로드될 때 단 한번만 수행되며, 주로 클래스변수(static 변수)를 초기화하는데 주로 사용

#### final
- 사용될 수 있는 곳
	- 클래스, 메서드, 멤버변수, 지역변수
- final 클래스 : 다른 클래스의 조상이 될 수 없음
- final 변수는 상수이므로 일반적으로 선언과 초기화를 동시에 하지만 인스턴스변수의 경우 생성자에서 초기화 가능

#### abstract
- 사용될 수 있는 곳
	- 클래스, 메서드

#### 접근제어자
- public > protected > default > private

|제어자|같은 클래스|같은 패키지|자손 클래스|전체|
|-------|-------------|--------------|-------------|-----|
|public| O | O | O | O |
|protected| O | O | O ||
|default| O | O |||
|private| O ||||

|대상|사용가능한 접근 제어자|
|-----|--------------------------|
|클래스|public, (default)|
|메서드, 멤버변수|public, protected, (default), private|
|지역변수|없음|

- 생성자가 private인 클래스는 다른 클래스의 조상이 될 수 없음, 클래스 앞에 final을 추가하여 상속할 수 없는 클래스임을 명시하는 것이 좋음

### 다형성

{% highlight java %}
class CastingTest {
	public static void main(String args[]) {
		Car car = new Car();
		FireEngine fe = null;
		fe = (FireEngine) car; // 실행 시 에러 발생
	}
}
{% endhighlight %}

- 컴파일은 성공하지만 car가 참조하고 있는 인스턴스가 Car 타입의 인스턴스, 즉 조상타입의 인스턴스를 자손타입의 참조변수로 참조하는 것은 허용하지 않기때문에 에러 발생

#### 참조변수와 인스턴스의 연결
- **멤버변수가 조상 클래스와 자손 클래스에 중복으로 정의된 경우, 조상 타입의 참조변수를 사용했을 때는 조상 클래스에 선언된 멤버변수가 사용되고, 자손타입의 참조변수를 사용했을 때는 자손 클래스에 선언된 멤버변수가 사용 됨**
- static메서드는 static변수처럼 참조변수의 타입에 영향을 받음, 참조변수의 타입에 영향을 받지 않는 것은 인스턴스 메서드 뿐

### 인터페이스
- 모든 멤버변수는 public static final 이어야 하며 생략 가능
- 모든 메서드는 public abstract 이어야 하며 생략 가능
- 오버라이딩 할 때 조상의 메서드보다 넓은 범위의 접근 제어자를 지정해야하기 때문에 인터페이스의 메서드는 public abstract가 생략된 것이므로 구현 메서드는 반드시 public으로 함

## Chapter8. 예외처리

### 예외처리
- 에러 : 프로그램 코드에 의해 수습될 수 없는 심각한 오류 ex) 메모리 부족, 스택 오버 플로우
- 예외 : 프로그램 코드에 의해 수습될 수 있는 다소 미약한 오류
- 예외는 RuntimeException클래스와 RuntimeException 제외 클래스로 니뉨
	- RuntimeException : 프로그래머의 실수로 발생하는 예외, 예외처리 하지 않아도 컴파일 문제 없음
	- RuntimeException 제외 : 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외, 예외처리 안할 시 컴파일 에러 발생
- try 또는 catch블럭에서 return문이 실행되는 경우에도 finally 블럭이 수행 됨

## Chapter9. java.lang패키지

### Object클래스

#### equals()
- String, Date, File, wrapper클래스는 주소값이 아닌 내용을 비교하도록 오버라이딩 됨, 의외로 StringBuffer클래스는 오버라이딩 되지 않음

#### hashCode()
- Object클래스에 정의된 hashCode()는 객체의 주소값을 이용해서 해시코드를 만들어 반환
- 클래스의 멤버변수 값으로 객체의 같고 다름을 판단해야하는 경우 hashCode메서드를 적절히 오버라이딩 해야 함
-  해싱기법을 사용하는 HashMap이나 HashSet과 같은 클래스에 저장할 객체라면 반드시 hashCode()를 오버라이딩해야 함.

#### clone()
- Object클래스에 정의된 clone메서드는 단순히 멤버변수의 값만을 복사하기 때문에 배열이나 인스턴스가 멤버로 정의되어 있는 클래스의 인스턴스는 완전한 복제가 이루어지지 않으므로 오버라이딩해서 새로운 배열을 생성하고 배열의 내용을 복사하도록 해야 함
- Cloneable 인터페이스를 구현한 클래스의 인스턴스만 clone()을 통한 복제가 가능, 인스턴스 복제는 데이터를 복사하는 것이기 때문에 데이터를 보호하기 위해서 클래스 작성자가 복제를 허용하는 경우에만 복제가 가능하도록 하기 위함
- 배열, Vector, ArrayList, LinkedList, HashSet, TreeSet, HashMap, TreeMap, Calendar, Date 복제 가능

### String클래스
- 리터럴로 문자열을 생성했을 경우, 같은 내용의 문자열들은 모두 하나의 String을 참조
- String 인스턴스의 경우 new 연산자에 의해 메모리 할당이 이루어지므로 항상 새로운 String 인스턴스가 생성
- String리터럴들은 컴파일 시에 클래스 파일에 저장 됨
- 모든 클래스 파일(*.class)에는 ‘constant pool’이라는 상수 목록이 있어서, 여기에 클래스 내에서 사용되는 모든 리터럴과 상수들이 저장 됨
- String 멤버변수를 따로 초기화 해주지 않으면 null로 초기화 되고 + 연산을 할 경우 참조변수를 “null”로 변환한 후 연산을 하므로 문자열간 결합에 문제가 될수 있음. String 변수의 선언과 함께 빈 문자열로 초기화 해주는 것이 좋음

#### intern()
- String인스턴스의 문자열을 ‘constant pool’에 등록. 등록하고자 하는 문자열이 ‘constant pool’에 이미 존재할 경우 그 문자열의 주소값을 반환
- intern()이 수행된 String 인스턴스는 등가비교연산자(==)를 가지고도 문자열 비교 가능
- equals 메서드로 문자열의 내용을 비교하는 것 보다 등가비교연산자(==)를 이용해서 주소(4 byte)를 비교하는 것이 빠르므로 intern메서드와 등가비교연산자(==)를 사용하기도 함

### wrapper클래스
- 문자열 -> 숫자

{% highlight java %}
int i = new Integer(“100”).intValue();
int i2 = Integer.parseInt(“100”);
Integer i3 = Integer.valueOf(“100”);
int i4 = Integer.valueOf(“100”); // autoboxing. 성능상 조금 더 느림
{% endhighlight %}

- 문자열(x진법) -> 숫자

{% highlight java %}
static int parseInt(String s, int radix) // 문자열 s를 radix 진법으로 인식
static Integer valueOf(String s, int radix)
{% endhighlight %}

## Chapter10. 내부 클래스
- 장점
	- 내부 클래스에서 외부 클래스의 멤버들을 쉽게 접근
	- 코드의 복잡성을 줄임(캡슐화)
- 종류와 특징

|내부 클래스|특징|
|-------------|-----|
|인스턴스 클래스|외부 클래스의 멤버변수 선언위치에 선언, 외부 클래스의 인스턴스멤버처럼 다뤄짐. 주로 외부 클래스의 인스턴스 멤버들과 관련된 작업에 사용|
|스태틱 클래스|외부 클래스의 멤버변수 선언위치에 선언, 외부 클래스의 static 멤버처럼 다뤄짐. 외부 클래스의 static멤버, 특히 static메서드에서 사용|
|지역 클래스|외부 클래스의 메서드나 초기화 블럭 안에서 선언, 선언된 영역 내부에서만 사용|
|익명 클래스| 클래스의 선언과 객체의 생성을 동시에 하는 이름없는 클래스(일회용)|

- 내부 클래스도 클래스이기 때문에 abstract나 final과 같은 접근 제어자를 사용할 수 있을 뿐 아니라 멤버변수들 처럼 private, protected 같은 접근 제어자도 사용 가능
- 지역클래스는 외부 클래스의 인스턴스멤버와 static멤버를 모두 사용할 수 있으며, 지역 클래스가 포함된 메서드에 정의된 지역변수도 사용할 수 있음. 단, final이 붙은 지역변수만 접근 가능하며 메서드가 수행을 마쳐서 지역변수가 소멸된 시점에도 지역 클래스의 인스턴스가 소멸된 지역변수를 참조하려는 경우가 발생 할 수 있기 때문
- 외부 클래스가 아닌 다른 클래스에서 내부 클래스 생성하고 내부 클래스의 멤버에 접근 하는 예제 (이런 상황이 발생했다는 것은 설계가 잘 못 되었다는 것)

{% highlight java %}
Outer oc = new Outer();
Outer.InstanceInner ii = oc.new InstanceInner();

ii.iv;
Outer.StaticInner.cv;

Outer.StaticInner si = new Outer.StaticInner();
si.iv;
{% endhighlight %}

- 내부 클래스와 외부 클래스에 선언된 변수의 이름이 같을 때 변수 앞에 ‘this’ 또는 ‘외부 클래스명.this’를 붙여 서로 구별 가능

## Chapter11. 컬렉션 프레임웍과 유용한 클래스

- Vector나 Hashtable과 같은 기존의 컬렉션 클래스들은 호환을 위해, 설계를 변경해서 남겨두었지만 가능하면 사용하지 않는 것이 좋음

### 컬렉션 프레임웍

#### 동기화
- 구버전의 Vector와 Hashtable은 자체적으로 동기화가 되는데 멀티쓰레드 프로그래밍이 아닌 경우 불필요한 기능이 되어 성능이 떨어짐
- 새로 추가된 ArrayList와 HashMap과 같은 컬렉션은 필요한 경우에만 java.util.Collections 클래스의 동기화 메서드 이용
- Collections클래스에는 다음과 같은 동기화 메서드를 제공

```
static Collection synchronizedCollection(Collection c)
static List synchronizedList(List list)
static Map synchronizedMap(Map m)
static SEt synchronizedSet(Set s)
static SortedMap synchronizedSortedMap(SortedMap m)
static SortedSet synchronizedSortedSet(SortedSet s)

List list = Collections.synchronizedList(new ArrayList(...));
```

#### Deep Copy vs Shallow Copy
- Deep Copy
	- 원본과 같은 데이터를 저장하고 있는 새로운 객체나 배열을 생성
- Shallow Copy
	- 단순히 참조만 복사

#### LinkedList
- 배열의 단점
	- 크기를 변경할 수 없음
	- 비순차적인 데이터의 추가/삭제에 시간이 많이 걸림
- 배열의 단점을 보완, 더블 셔쿨러 링크드리스트로 구현
- 순차적으로 추가/삭제하는 경우 ArrayList가 LinkedList보다 빠름
- 중간 데이터를 추가/삭제하는 경우 LinkedList가 ArrayList보다 빠름

#### Iterator
- 컬렉션에 저장된 요소들을 읽어오는 방법을 표준화 함

#### ListIterator
- Iterator를 상속받아서 기능을 추가한 것으로, 컬렉션의 요소에 접근할 때 양방향으로 이동이 가능

#### HashSet
- hashCode 조건
	- 실행 중인 어플리케이션 내의 동일한 객체에 대해서 여러번 hashCode()를 호출해도 동일한 int값을 반환 해야함. 하지만 실행시마다 동일한 int값을 반환할 필요는 없음. 단, equals메서드의 구현에 사용된 멤버변수의 값이 바뀌지 않았다고 가정
	- equals메서드를 이용한 비교에 의해서 true를 얻은 두 객체에 대해 각각 hashCode()를 호출해서 얻은 결과는 반드시 같아야 함
	- equals메서드를 호출했을 때 false를 반환하는 두 객체는 hashCode() 호출에 대해 같은 int값을 반환하는 경우가 있어도 괜찮지만, 해싱을 사용하는 컬렉션의 성능을 향상시키기 위해서는 다른 int값을 반환하는 것이 좋음

##### 해싱
- 해시함수를 이용해서 데이터를 해시테이블에 저장하고 검색하는 기법
- 해시함수는 데이터가 저장되어 있는 곳을 알려주기때문에 다량의 데이터 중에서도 원하는 데이터를 빠르게 찾음
- 배열과 링크드리스트의 조합으로 되어있음
- 저장할 데이터의 키를 해시함수에 넣으면 배열의 한 요소를 얻게되고, 다시 그 곳에 연결되어 있는 링크드리스트에 저장
- 해싱을 구현하는 과정에서 제일 중요한것은 해시함수의 알고리즘 임
- HashMap과 같이 해싱을 구현한 컬렉션 클래스에서는 Object클래스에 정의된 hashCpde()를 해시함수로 사용
- Object클래스에 정의된 hashCode()는 객체의 주소를 이용하는 알고리즘으로 해시코드를 만들어 내기 때문에 모든 객체에 대해 hashCode()를 호출한 결과가 유일한 훌륭한 방법

#### TreeSet
- 이진검색트리(정렬, 검색, 범위검색에 뛰어난 성능)라는 자료구조 형태로 데이터를 저장하는 컬렉션 클래스
- 이진검색트리의 성능을 향상시킨 ‘레드-블랙 트리’로 구현
- 이진검색트리
	- 모든 노드는 최대 두 개의 자식노드를 가짐
	- 왼쪽 자식노드의 값은 부모노드의 값보다 작고 오른쪽자식노드의 값은 부모노드의 값보다 큼
	- 노드의 추가 삭제에 시간이 걸림
	- 검색과 정렬에 유리
- TreeSet을 생성할 때 Comparator를 지정해주지 않으면 저장하는 객체에 구현된 정렬방식에 따라 정렬하여 저장, Comparator를 지정해주면 지정된 Comparator에 구현된 정렬방식에 따라 정렬하여 저장

#### Comparator와 Comparable
- 객체들을 정렬 또는 이진검색트리를 구성하는데 필요한 메서드를 정의하는 인터페이스
- TreeSet, 정렬 등은 Comparable을 구현한 내용에 따라 정렬
- java.lang.Comparable : 기본 정렬기준을 구현하는데 사용
- java.util.Comparator : 기본 정렬기준 외에 다른 기준으로 정렬하고자 할 때 사용

#### Properties
- store()로 저장할 경우 한글이 unicode로 바뀜
- storeToXML()을 이용할 경우 한글이 그대로 저장돼 한글편집이 가능

### 유용한 클래스들

#### Calendar와 Date
```
// Calendar를 Date로 변환
Calendar cal = Calendar.getInstance();
...
Date d = new Date(cal.getTimeInMillis());

// Date를 Calendar로 변환
Date d = new Date();
...
Calendar cal = Calendar.getInstance();
cal.setTime(d);
```

#### Random, 정규식(Pattern, Match), Scanner, StringTokenizer...

### 형식화 클래스

#### DecimalFormat
- 숫자 데이터를 정수, 부동소수점, 금액등의 다양한 형식으로 표현, 반대로 텍스트 데이터를 숫자로 쉽게 변환

#### SimpleDateFormat
#### ChoiceFormat
#### MessageFormat

### 제네릭스
- public static <T extends Comparable<? super T>> void sort(List<T> list)
	- T가 Student타입이고 Student타입이 Person의 자손이라면 <Student extends Comparable<Student>> 또는 <Student extends Comparable<Person>>이 가능함

## Chapter12. 쓰레드

### start()와 run()
1. main 메서드에서 쓰레드의 start메서드를 호출한다.
2. start메서드는 쓰레드가 작업을 수행하는데 사용될 새로운 호출스택을 생성한다.
3. 생성된 호출스택에 run메서드를 호출해서 쓰레드가 작업을 수행하도록 한다.
4. 이제는 호출스택이 2개이기 때문에 스케줄러가 정한 순서에 의해서 번갈아 가면서 실행된다.

### 쓰레드의 실행제어

#### 쓰레드의 상태

|상태|설명|
|----|------|
|NEW|쓰레드가 생성되고 아직 start()가 호출되지 않은 상태|
|RUNNABLE|실행 중 또는 실행 가능한 상태|
|BLOCKED|동기화블럭에 의해서 일시정지된 상태(lock이 풀릴 때까지 기다리는 상태)|
|WAITING, TIMED_WAITING|쓰레드의 작업이 종료되지는 않았지만 실행가능하지 않은(unrunnable) 일시정지상태. TIMED_WAITING은 일시정지시간이 지정된경우를 의미|
|TERMINATED|쓰레드의 작업이 종료된상태|

#### 쓰레드의 스케줄링과 관련된 메서드
|생성자/메서드|설명|
|----------------|-----|
|void interrupt()|sleep()이나 join()에 의해 일시정지상태인 쓰레드를 실행대기상태로 만듦. 해당 쓰레드에서 InterruptedException이 발생하므로써 일시정지상태를 벗어나게 됨|
|void join()..| 지정된 시간동안 쓰레드가 실행되도록 함. 지정된 시간이 지나거나 작업이 종료되면 join()을 호출한 쓰레드로 다시 돌아와 실행을 계속 함|
|~~void resume()~~|suspend()에 의해 일시정지상태에 있는 쓰레드를 실행대기 상태로 만듦|
|static void sleep(long millis) ... | 지정된 시간동안 쓰레드를 일시정지 시킴. 지정한 시간이 지나면 자동적으로 다시 실행대기상태가 됨. sleep()은 항상 현재 실행 중인 쓰레드에 대해 작동함(?). 특정쓰레드에 sleep을 호출하기 보단 Thread.sleep(5000)과 같이 호출해야 하는게 알맞음.|
|~~void stop()~~|쓰레드를 즉시 종료시킴. 교착상태에 빠지기 쉽게 때문에 deprecated 됨|
|~~void suspend()~~|쓰레드를 일시정지시킴. resume()을 호출하면 다시 실행대기 상태가 됨|
|static void yield()|실행 중에 다른 쓰레드에게 양보하고 실행대기상태가 됨|

### wait()과 notify()
- 한 쓰레드가 객체에 lock을 걸고 어떤 조건이 만족될 때까지 기다려야하는 경우 이 쓰레드를 그대로 놔두면 이 객체를 사용하려는 다른 쓰레드들은 lock이 풀릴때까지 기다려야하는 비효율을 개선하기 위해 사용
- wait
	- 쓰레드가 wait()을 호출하면 자신이 객체에 걸어 놓았던 모든 lock을 풀고, wait()이 호출된 객체의 대기실에서 기다림. 다른 쓰레드에 의해 그 객체에 대해 notify()를 호출하면 객체의 대기실을 벗어나서 다시 실행대기상태가 됨.
- notify, notifyAll
	- 쓰레드를 깨움
- waiting pool은 객체마다 존재

## Chapter14. 입출력

### 자바에서의 입출력
- 입출력을 위해 데이터를 전달하려면 두 대상을 연결하고 데이터를 전송할 수 있는 무언가가 필요한데 이것을 스트림이라고 정의
- 스트림
	- 데이터를 운반하는데 사용되는 연결통로

### 바이트기반 스트림
- 입력스트림과 출력스트림의 종류

|입력스트림|출력스트림|입출력 대상 종류|
|------------|-------------|-------------------|
|FileInputStream|FileOutputStream|파일|
|ByteArrayInputStream|ByteArrayOutputStream|메모리(byte배열)|
|PipedInputStream|PipedOutputStream|프로세스(프로세스간의 통신)|
|AudioInputStream|AudioOutputStream|오디오장치|

- InputStream, OutputStream의 자손이며 각각 읽고 쓰는데 필요한 추상메서드를 자신에 맞게 구현

- InputStream과 OutputStream에 정의된 읽기와 쓰기를 수행하는 메서드

|InputStream|OuputStream|
|----------------|-----------------|
|abstract int read()|abstract void write(int b)|
|int read(byte[] b)|void write(byte[] b)|
|int read(byte[] b, int off, int len)|void write(byte[] b, int off, int len)|

#### ByteArrayInputStream과 ByteArrayOutputStream
- 메모리, 즉 바이트배열에 데이터를 입출력하는데 사용되는 스트림
- 주로 다른 곳에 입출력하기 전에 데이터를 임시로 바이트배열에 담아 변환 등의 작업을 하는데 사용
- 바이트배열이 사용하는 메모리 자원은 가비지컬렉터에 의해 자동적으로 반환되므로 close()를 이용해 스트림을 닫지 않아도 됨
```
while (input.available() > 0) {
	int len = input.read(temp);
	ouput.write(temp, 0, len);
}
```

### 바이트기반의 보조스트림
#### FilterInputStream과 FilterOutputStream
- InputStream/Output의 자손이면서 모든 보조스트림의 조상
- 보조스트림은 자체적으로 입출력을 수행할수 없기 때문에 기반스트림을 필요로 함

#### BufferedInputStream과 BufferedOutputStream
- BufferedInputStream
	- 버퍼크기는 입력소스로부터 한 번에 가져올 수 있는 데이터의 크기로 지정하면 좋음
	- 프로그램에서 입력소스로부터 데이터를 읽기 위해 처음으로 read메서드를 호출하면, BufferedInputStream은 입력소스로 부터 버퍼 크기만큼의 데이터를 읽어다 자신의 내부버퍼에 저장. 프로그램에서는 BufferedInputStream의 버퍼에 저장된 데이터를 읽음. 외부의 입력소스로부터 읽는 것보다 내부의 버퍼로부터 읽는 것이 훨씬 빠르기 때문에 효율이 높아짐. 프로그램에서 버퍼에 저장된 모든 데이터를 다 읽고 다음 데이터를 읽기위해 read메서드가 호출되면 BufferedInputStream은 입력소스로부터 다시 버퍼크기만큼의 데이터를 읽어다 버퍼에 저장함

- BufferedOutputStream
	- 프로그램에서 write메서드를 이용한 출력이 BufferedOutputStream의 버퍼에 저장. 버퍼가 가득 차면 버퍼의 모든 내용을 출력 소스에 출력. 버퍼를 비우고 다시 프로그램으로부터 출력을 저장할 준비를 함.프로그램에서 모든 출력작업을 마친 후 BufferedOutputStream에 close()나 flush()를 호출해서 마지막에 버퍼에 있는 모든 내용이 출력소스에 출력되도록 해야 함

#### DataInputStream과 DataOutputStream
- byte 단위가 아닌 8가지 기본 자료형의단위로 읽고 쓸 수 있음

#### SequenceInputStream
-  여러개의 입력스트림을 연속적으로 연결해서 하나의 스트림으로부터 데이터를 읽는 것과 같이 처리

#### PrintStream
- 데이터를 적절한 문자로 출력
- PrintWriter로 대체

### 문자기반 스트림
#### Reader와 Writer
- byte 배열대신 char배열을 사용하며 여러종류의 인코딩과 자바에서 사용하는 유니코드간의 변환을 자동으로 처리 함

#### FileReader와 FileWriter
- 파일로부터 텍스트 데이터를 읽고, 파일에 쓰는데 사용

#### PipedReader와 PipedWriter
- 쓰레드간에 데이터를 주고받을 때 사용
- 다른 스트림과 달리 입력과 출력스트림을 하나의 스트림으로 연결해서 데이터를 주고 받음
- 스트림을 생성한 다음 어느 한쪽 쓰레드에서 connect()를 호출해서 입력스트림과 출력스트림을 연결

#### StringReader와 StringWriter
- StringWriter에출력되는 데이터는 내부의 StringBuffer에 저장되며 StringWriter의 다음과 같은 메서드를 이용해서 제정된 데이터를 얻을 수 있음
	- StringBuffer getBuffer() : StringWriter에 출력한 데이터가 저장된 StringBuffer를 반환
	- String toString() : StringWriter에 출력된 문자열을 반환

### 문자기반의 보조스트림
#### BufferedReader와 BufferedWriter
- readLine()을 사용해 데이터를 라인단위로 읽어올 수 있음

#### InputStreamReader와 OutputStreamWriter
- 바이트기반 스트림을 문자기반 스트림으로 연결시켜주는 역할
- 바이트기반 스트림의 데이터를 지정된 인코딩의 문자데이터로 변환하는 작업을 수행

### 표준입출력과 File
#### 표준입출력 - System.in, System.out, System.err
- 표준입출력은 콘솔을 통한 데이터 입력과 콘솔로의 데이터 출력을 의미 함

#### 표준입출력의 대상변경 - setOut(), setErr(), setIn()
- 입출력을 콘솔 외에 다른 입출력 대상으로 변경하는 것이 가능

#### RandomAccessFile
- 하나의 클래스로 파일에 대한 입력과 출력을 모두 할 수 있음
- 다른 입출력 클래스들은 입출력소스에 순차적으로 읽기/쓰기를 하기 때문에 읽기와 쓰기가 제한적인데 반해 RandomAccessFile클래스는 파일포인터를 사용하여 파일에 읽고쓰는 위치에 제한이 없음


### 직렬화
- 객체를 데이터 스트림으로 만드는 것

#### ObjectInputStream, ObjectOutputStream
```
FileOutputStream fos = new FileOutputStream(“objectfile.ser”);
ObjectOutputStream out = new ObjectOutputStream(fos);

out.writeObject(new UserInfo());

FileInputStream fis = new FileInputStream(“objectfile.ser”);
ObjectInputStream in = new ObjectInputStream(fis);

UserInfo info = (UserInfo) in.readObject();
```

- 자동 직렬화가 편리하기는 하지만 오래 걸림. 직렬화 작업시간을 단축시키려면 직렬화하고자 하는 객체의 클래스에 추가적으로 writeObject, readObject 메서드를 직접 구현해주어야 함

#### 직렬화가 가능한 클래스 만들기 - Serializable, transient
- Serializable 인터페이스를 구현하면 직렬화 가능
- Serializable을 구현한 클래스를 상속받는다면 Serializable을 구현하지 않아도 직렬화 가능
- 조상클래스가 Serializable을 구현하지 않았다면 자손클래스를 직렬화할때 조상클래스에 정의된 인스턴스변수는 직렬화 대상에서 제외. 추가 구현을 통해 가능하게 할 수 있음
- Serializable을 구현하지 않은 객체를 참조할 경우 질렬화 할 수 없음. transient를 사용하여 질려화 대상에서 제외할 수 있음