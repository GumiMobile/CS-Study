## JVM 작동방식 및 Java의 메모리 영역

## 우지현

### JVM (Java Virtual Machine)

- 스택 기반의 가상머신

- 자바 애플리케이션을 클래스 로더를 통해 읽어들여 자바 API와 함께 실행시킨다.

- Java와 OS 사이에서 중개자 역할을 수행하여 Java가 OS에 구애받지 않고 재사용을 가능하게 해준다.

- 메모리 관리, Garbage Collection 수행

  

### Java 프로그램 실행 과정

![자바 프로그램 실행 단계](https://t1.daumcdn.net/cfile/tistory/99960C505C37390616)

1. 프로그램이 실행되면 JVM은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당받는다.

   JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.

2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어들여 자바 바이트코드(.class)로 변환시킨다.

3. Class Loader를 통해 class 파일들을 JVM으로 로딩한다.

4. 로딩된 class 파일들은 Exceution engine을 통해 해석된다.

5. 해석된 바이트코드는 Runtime Data Area에 배치되어 실질적인 수행이 이루어지게 된다.

이러한 실행 과정 속에서 JVM은 필요에 따라 Thread Synchrnoization과 GC 같은 관리작업을 수행한다.

### Runtime Data Area

![Runtime Data Area](https://t1.daumcdn.net/cfile/tistory/992EE9465D08E9B903)

- JVM의 메모리 영역
- 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역

#### PC Register

- Thread가 시작될 때 생성되며 생성될 때마다 생성되는 공간으로 스레드마다 하나씩 존재
- Thread가 어떤 부분을 어떤 명령으로 실행해야할지에 대한 기록을 하는 부분
- 현재 수행 중인 JVM 명령의 주소를 가지고 있다.

#### JVM Stack

- 프로그램 실행 과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역
- 각종 형태의 변수나 임시 데이터, 스레드나 메소드의 정보 저장
- 메소드 호출 시마다 각각의 스택 프레임(그 메소드만을 위한 공간)이 생성된다.
- 메소드 수행이 끝나면 프레임별로 삭제
- 호출된 메소드의 매개변수, 지역변수, 리턴값 및 연산 시 일어나는 값들을 임시로 저장

#### Native Method Stack

- 자바 프로그램이 컴파일되어 생성되는 바이트코드가 아닌 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역
- Java가 아닌 다른 언어로 작성된 코드를 위한 공간
- Java Native Interface를 통해 바이트코드로 전환하여 저장
- 일반 프로그램처럼 커널이 스택을 잡아 독자적으로 프로그램을 실행시킨다.
- 이 부분을 통해 C 코드를 실행시켜 커널에 접근 가능

#### Method Area (= Class Area, Static Area)

- 클래스 정보를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간
- 올라가게 되는 메소드의 바이트코드는 프로그램의 흐름을 구성하는 바이트코드
  - 자바 프로그램은 main 메소드의 호출에서부터 계속된 메소드의 호출로 흐름을 이어가기 때문
  - 대부분의 인스턴스 생성도 메소드 내에서 명령하고 호출한다.
  - 사실상 컴파일된 바이트코드의 대부분이 메소드 바이트코드이기 때문에 거의 모든 바이트코드가 올라간다고 봐도 상관없다.
- Runtime Constant Pool이라는 별도의 관리 영역도 함께 존재
  - 상수 자료형을 저장하여 참조하고 중복을 막는 역할 수행
- 올라가는 정보의 종류
  - Field Information
    - 멤버변수의 이름, 데이터 타입, 접근 제어자에 대한 정보
  - Method Information
    - 메소드의 이름, 리턴타입, 매개변수, 접근 제어자에 대한 정보
  - Type Information
    - class인지 interface인지 여부 저장/Type 속성, 전체 이름, super class의 전체 이름(interface이거나 object인 경우 제외)
- GC의 관리 대상에 포함된다.

#### Heap

아래 그림은 Java 8이전까지의 Heap 메모리 구조이다.

![Heap](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKA8I0%2Fbtq42yTNQ0q%2F855nAKHEBJP8fFLkPAzjS0%2Fimg.png)

- 객체를 저장하는 가상 메모리 공간
- Method Area가 클래스 데이터를 위한 공간이라면 Heap은 객체를 위한 공간
- new 연산자로 생성된 객체와 배열 저장
- 물론 class area 영역에 올라온 클래스들만 객체로 생성 가능
- Heap은 세 부분으로 나눌 수 있다.
  - Permanent Generation
    - 생성된 객체들의 정보의 주소값이 저장된 공간
    - Class Loader에 의해 load되는 클래스, 메소드 등에 대한 메타 정보가 저장되는 영역이고 JVM에 의해 사용된다.
    - Reflection을 사용하여 동적으로 클래스가 로딩되는 경우에 사용된다.
    - 내부적으로 Reflection 기능을 자주 사용하는 Spring Framework를 이용할 경우 이 영역에 대한 고려가 필요하다.
  - New/Young Generation
    - `Eden` : 객체들이 최초로 생성되는 공간
    - `Survivor 1 / 2` : Eden에서 참조되는 객체들이 저장되는 공간
    - `Eden` 영역에 객체가 가득 차게 되면 첫번째 GC(minor GC) 발생
    - `Eden` 영역에 있는 값들을 `Survivor 1` 또는 `Survivor 2` 영역에 복사하고 해당 영역을 제외한 나머지 영역의 객체 삭제
    - `Survivor 1`과 `Survivor 2` 둘 중 하나는 항상 공간이 비워진 채로 유지된다.
  - Old Generation
    - Young 영역에서 일정 시간 참조되고 있는, 살아남은 객체들이 저장되는 공간
    - 보통 Young 영역보다 크게 할당하며, 이러한 이유로 Old 영역의 GC는 Young 영역보다 적게 발생한다.
    - Old 영역에서는 2차 GC인 Major GC(Full GC)가 일어나게 되며, GC를 진행하는 Thread를 제외하고 이외의 모든 Thread를 멈춘 상태로 GC가 진행된다 (`Stop-the-world`)

## 김민수

### 자바 코드 실행 과정

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99A840415B739DF119" style width="400"/>

1. 자바 소스(.java) 파일은 자바 컴파일러를 통해 자바 바이트 코드(.class)로 컴파일 된다.
2. 컴파일 된 바이트 코드는 JVM의 클래스 로더에게 전달된다.
3. 클래스 로더는 동적로딩을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터 영역, 즉 JVM의 메모리에 올린다.
4. 실행 엔진은 JVM 메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행한다.



### JVM의 구성 및 동작원리

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F990BF73B5B73A02817" style width="400"/>

#### 클래스 로더

클래스 로더의 특징

1. 계층 구조

   <img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99DBA04D5B73A37321" style width="400"/>

   - 부트스트랩 클래스 로더
     - 최상위 클래스 로더로 JAVA가 아닌 네이티브 코드로 구현
     - JVM이 실행될 때 메모리에 올라감
     - Object 클래스 등 JAVA API 로드
   - 익스텐션 클래스 로더
     - 기본 JAVA API를 제외한 확장 클래스 로드
   - 시스템 클래스 로더
     - 부트스트랩, 익스텐션 클래스 로더가 JVM 자체의 구성요소를 로드한다면, 시스템 클래스 로더는 어플리케이션의 클래스를 로드
     - 사용자가 지정한 $Classpath 내의 클래스들을 로드
   - 사용자 정의 클래스 로더
     - 어플리케이션 사용자가 직접 코드상에서 생성하여 사용하는 클래스 로더

2. 위임 모델

   <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99644E3E5B73BA5D36" style width="400"/>

   처음 바이트 코드를 넘겨받은 클래스 로더가 필요한 클래스를 로드할 때, 혹은 실행 엔진에서 처음으로 참조하는 클래스를 클래스 로더에게 요청할 때 위임 모델은 다음 순서대로 클래스가 있는지 확인한다.

   1. 클래스 로더 캐시
   2. 상위 클래스 로더 이때, 중간에 클래스를 찾았더라도 부트스트랩 클래스 로더까지 올라감.
   3. 자기 자신이 파일 시스템에서 클래스 탐색 (실패 -> 에러 발생)

3. 가시성 제한

   클래스 로더가 로드를 요청 받았을 때 위임 모델에 의해 클래스 로더 캐시를 확인하고 없으면 상위 클래스 로더를 확인한다. 이때, 하위 클래스 로더에 있는 클래스는 확인 불가능한 특성이 가시성 제한이다.

4. 언로드 불가

   클래스를 로드하는 것은 가능하지만 언로드하는 것은 불가능하다.

5. 네임 스페이스

   각 클래스 로더들이 가지고 있는 공간으로써 로드된 클래스를 보관하는 공간이며, 클래스를 로드할 때 위임 모델을 통해 상위 클래스 로더의 네임 스페이스를 확인한다. 네임 스페이스에 보관되는 기준은 FQCN(Fully Qualified Class Name)으로 이는 패키지명까지 포함하는 식별자이다.

   클래스 로더들은 각자 네임 스페이스를 가지고 있어 FQCN이 같은 클래스더라도 네임 스페이스가 다르면 다른 클래스로 간주한다.

#### 런타임 데이터 영역

JVM이 OS 위에서 실행되면서 할당받는 메모리 영역이다. 5가지 영역으로 나눌 수 있다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99B467465B73D15111" style width="400"/>

위 그림에서 보라색 영역은 스레드마다 생성되고, 힙, 메서드 영역은 스레드가 공유해서 사용한다.

- PC Register

  Program Counter Register는 현재 수행 중이 명령의 주소를 가지며 스레드가 시작될 때 생성된다.

- JVM Stack

  Stack Frame이라는 구조체를 저장하는 스택이다. 예외 발생 시, printStackTrae() 메서드로 보여주는 Stack Trace의 라인들이 스택 프레임을 표현한다.

- Native Method Stack

  JAVA 외의 언어로 작성된 네이티브 코드를 위한 스택이다. JNI(Java Native Interface)를 통해 호출하는 C/C++ 등의 코드를 수행하기 위한 스택으로, 언어에 맞게 생성된다.

- Heap

  인스턴스, 객체를 저장하는 공간으로 GC의 대상이다. JVM 성능 이슈에서 가장 많이 언급되는 공간이다. 힙 구성, GC 방법은 JVM 벤더들의 소관이다.

- Method Area

  모든 스레드가 공유하는 영역으로 JVM이 시작될 때 생성된다. JVM이 읽어 들인 각각의 클래스와 인터페이스에 대한 런타임 상수 풀, 필드와 메서드에 대한 정보, static 변수, 메서드의 바이트 코드 등을 저장한다.

  > 런타임 상수 풀(Runtime constant pool)
  >
  > JVM 동작에서 가장 핵심적인 역할을 수행하는 곳으로 중요하다. 각 클래스와 인터페이스의 상수 뿐만 아니라, 메서드와 필드에 대한 모든 레퍼런스까지 갖고 있는 테이블로, 어떤 메서드나 필드를 참조할 때, JVM은 런타임 상수 풀을 통해 해당 메서드나 필드의 실제 메모리 주소를 찾는다.

#### 실행 엔진

클래스 로더를 통해 런타임 데이터 영역에 배치된 바이트 코드를 명령어 단위로 읽어 실행한다. 바이트 코드의 각 명령어는 1바이트 크기의 Operation Code와 추가 피연산자로 이뤄져 있다. Operation Code를 기계가 실행할 수 있는 형태로 변환하는데 2가지 방법이 있다.

- 인터프리터

  바이트 코드 명령어를 하나씩 읽고 해석하여 실행한다. 명령어 하나의 해석은 빠르나 전체 실행 속도는 느리다. JVM안에서 바이트 코드는 일반적으로 인터프리터 형태로 실행된다.

- JIT 컴파일러 (Just In time Compiler)

  인터프리터의 단점을 보완하는 기술로, 바이트 코드 전체를 컴파일하여 네이티브 코드로 변경한다. 변경 후, 인터프리팅 하지 않고 네이티브 코드로 직접 실행한다. 변환된 네이티브 코드는 캐시에 보관하기 때문에 실행속도는 빠르지만 변환 과정이  인터프리팅 하는 것보다 오래 걸린다. 따라서 사용 횟수 기준을 넘은 메서드만 JIT 컴파일러를 통해 변환한다. 변환과정은 IR(Intermediate Representaion)을 거친 후, 최적화를 수행하고 네이티브 코드로 변환된다.

  > Intermediate Representaion
  >
  > - 소스 코드를 표현하기 위해 컴파일러 또는 가상 시스템에서 내부적으로 사용하는 데이터 구조 또는 코드이다.
  >
  > - IR은 최적화와 인터프리팅 등 추가 처리를 위해 도움이 되도록 설계되었다.
  > - 정보 손실 없이 소스 코드를 표현 가능하며, 특정 소스, 언어와 무관하게 코드를 나타낼 수 있다.


## 이수형

### JVM

자바 가상머신으로 자바 바이트 코드를 실행할 수 있는 주체<br/>
Class Loader, Excution Engine, GC, Runtime Data Area로 나뉜다

- 스택기반의 가상 머신
- GC 사용
- 자바를 OS에 구애받지않고 실행시킬 수 있는 주체
- 모든 기본 타입의 정의를 명확히 함으로써 플랫폼 독립성 보장

#### 실행과정

1. 자바 컴파일러가 소스코드를 읽어들여 .class파일로 변환시킨다
2. class loader를 통해서 class 파일을 JVM으로 로딩한다
3. 로딩된 class파일을 Execution engine을 통해 해석한다
4. 해석된 바이트코드를 Runtime Data Areas에 배치되어 수행된다.



#### class loader

클래스로더는 class파일을 읽어서 바이트 코드를 Methode Aread에 저장한다.<br/>
런타임 시에 클래스를 처음으로 참조할 때 해당 클래스를 로드하고 링크한다.

### Runtime Data Area

- Method area
   - 클래스정보를 처음 메모리에 올릴때 초기화대는 대상을 저장
   - 모든 클래스 정보와 static 변수를 저장한다
   > 멤버면수, 데이터 타입, 리턴타입, 파라미터 같은 메소드정보
   > Type 정보, constant Pool, final class 변수

- Heap area
   - 모든 객체를 저장하는 가상 메모리 공간
   - new 연산자로 생성된 객체와 배열 저장
   - Method area에 로드된 클래스만 생성 가능
   - GC가 참조되지 않는 메모리를 확인하고 제거

- JVM Stack area
   - 프로그램 실행과정에서 임시로 할당되었다가 메소드가 종료되면 소멸
   - 각종형태의 변수, 임시데이터, 메소드를 저장

- PC Registers
   - 쓰레드가 어떤 부분을 명령으로 실행 해야할지에 대한 기록
   - 현재 수행중인 JVM 명령의 주소를 가짐

- Native Method Stacks
   - 기계어로 컴파일된 자바 프로그램을 실행시키는 영역
   - 커널이 스택을 잡아 독자적으로 실행시킨다.
   - 보통 C/C++을 수행하기 위한 Stack

Method, Heap area는 모든 쓰레드에서 공유하고 Stack, PC Register, Native Method Stack area는 각 쓰레드별로 생성됨

#### Excution Engine

Class Loader에 의해 메모리에 적재된 바이트 코드들을 기계어로 변경해 명령어 단위로 실행
- 인터프리터 방식
   - 명령어를 하나하나 실행
- Just-In-Time(JIT) 방식
   - 적절한 시간에 전체 바이트 코드를 네이티브 코드로 변경해서 Excution Engine이 컴파일된 코드를 실행
   - 성능이 높다

## 김현수

### JVM(Java Virtual Machine)
- 자바 가상 머신으로 자바 바이트 코드를 실행할 수 있는 주체다.
- CPU나 운영체제(플랫폼)의 종류와 무관하게 실행이 가능하다.
- 즉, 운영체제 위에서 동작하는 프로세스로 자바 코드를 컴파일해서 얻은 바이트 코드를 해당 운영체제가 이해할 수 있는 기계어로 바꿔 실행시켜주는 역할을 한다.
- 크게 Class Loader, Execution Engine, Garbage Collector, Runtime Data Area로 구성된다.

![구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile22.uf.tistory.com%2Fimage%2F9973563D5ACE0315215FF6)

1. Class Loader

    자바에서 소스를 작성하면 Person.java 처럼 .java파일이 생성된다.

    .java 소스를 자바컴파일러가 컴파일하면 Person.class 같은 .class파일(바이트코드)이 생성된다.

    이렇게 생성된 클래스파일들을 엮어서 JVM이 운영체제로부터 할당받은 메모리영역인 Runtime Data Area로 적재하는 역할을 Class Loader가 한다. (자바 애플리케이션이 실행중일 때 이런 작업이 수행된다.)

2. Execution Engine

    Class Loader에 의해 메모리에 적재된 클래스(바이트 코드)들을 기계어로 변경해 명령어 단위로 실행하는 역할을 한다.

    명령어를 하나 하나 실행하는 인터프리터(Interpreter)방식이 있고 JIT(Just-In-Time) 컴파일러를 이용하는 방식이 있다.

    JIT 컴파일러는 적절한 시간에 전체 바이트 코드를 네이티브 코드로 변경해서 Execution Engine이 네이티브로 컴파일된 코드를 실행하는 것으로 성능을 높이는 방식이다.

3. Garbage Collector

    Garbage Collector(GC)는 Heap 메모리 영역에 생성(적재)된 객체들 중에 참조되지 않는 객체들을 탐색 후 제거하는 역할을 한다.

    GC가 역할을 하는 시간은 정확히 언제인지를 알 수 없다. (참조가 없어지자마자 해제되는 것을 보장하지 않음)

    또 다른 특징은 GC가 수행되는 동안 GC를 수행하는 쓰레드가 아닌 다른 모든 쓰레드가 일시정지된다.

    특히 Full GC가 일어나서 수 초간 모든 쓰레드가 정지한다면 장애로 이어지는 치명적인 문제가 생길 수 있는 것이다. (GC와 관련된 내용은 아래 Heap영역 메모리를 설명할 때 더 자세히 알아본다.)

4. Runtime Data Area

    JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역이다.

    이 영역은 크게 Method Area, Heap Area, Stack Area, PC Register, Native Method Stack로 나눌 수 있다.


### 자바 런타임 메모리(Runtime Data area)구조

![자바 런타임 메모리 구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile4.uf.tistory.com%2Fimage%2F99AED1445ACE0E4B04A102)

1. Method area (메소드 영역)

      클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보같은 필드 정보와 메소드의 이름, 리턴 타입, 파라미터, 접근 제어자 정보같은 메소드 정보, Type정보(Interface인지 class인지), Constant Pool(상수 풀 : 문자 상수, 타입, 필드, 객체 참조가 저장됨), static 변수, final class 변수등이 생성되는 영역이다.

2. Heap area (힙 영역)

    new 키워드로 생성된 객체와 배열이 생성되는 영역이다.

    메소드 영역에 로드된 클래스만 생성이 가능하고 Garbage Collector가 참조되지 않는 메모리를 확인하고 제거하는 영역이다.

3. Stack area (스택 영역)

    지역 변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값등이 생성되는 영역이다.

    int a = 10; 이라는 소스를 작성했다면 정수값이 할당될 수 있는 메모리공간을 a라고 잡아두고 그 메모리 영역에 값이 10이 들어간다. 즉, 스택에 메모리에 이름이 a라고 붙여주고 값이 10인 메모리 공간을 만든다.

    클래스 Person p = new Person(); 이라는 소스를 작성했다면 Person p는 스택 영역에 생성되고 new로 생성된 Person 클래스의 인스턴스는 힙 영역에 생성된다.

    그리고 스택영역에 생성된 p의 값으로 힙 영역의 주소값을 가지고 있다. 즉, 스택 영역에 생성된 p가 힙 영역에 생성된 객체를 가리키고(참조하고) 있는 것이다.

    메소드를 호출할 때마다 개별적으로 스택이 생성된다.

4. PC Register (PC 레지스터)

    Thread(쓰레드)가 생성될 때마다 생성되는 영역으로 Program Counter 즉, 현재 쓰레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역이다. (*CPU의 레지스터와 다름)

    이것을 이용해서 쓰레드를 돌아가면서 수행할 수 있게 한다.

5. Native method stack

    자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역이다.

    보통 C/C++등의 코드를 수행하기 위한 스택이다. (JNI)



> 쓰레드가 생성되었을 때 기준으로
> 
> 1,2번인 메소드 영역과 힙 영역을 모든 쓰레드가 공유하고,
> 
> 3,4,5번인 스택 영역과 PC 레지스터, Native method stack은 각각의 쓰레드마다 생성되고 공유되지 않는다.
