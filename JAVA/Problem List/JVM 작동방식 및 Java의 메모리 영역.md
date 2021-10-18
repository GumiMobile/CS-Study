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

