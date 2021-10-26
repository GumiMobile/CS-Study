# JAVA

- [객체 지향 프로그래밍이란 무엇인가?](#객체-지향-프로그래밍이란-무엇인가)

- [Garbage Collection](#Garbage-Collection)

- [SOLID](#SOLID)

- [Serialization (직렬화)](#serialization-직렬화)

  - [Deserialization (역직렬화)](#deserialization-역직렬화)
  - [SerialVersionUID](#serialversionuid)
- [JVM 작동방식 및 Java의 메모리 영역](#JVM-작동방식-및-Java의-메모리-영역)
  <br><br>

## 객체 지향 프로그래밍이란 무엇인가?

### Object Oriented Programming (OOP)

프로그래밍 패러다임 중 하나로, 현실 세계의 사물들을 기능을 제공하는 객체로 표현하여 프로그래밍 하는 것을 말한다. 프로그래밍에서 필요한 데이터를 추상화하여 상태와 행위를 가진 객체 형태로 나누고, 이 객체들 간의 상호작용을 통해 로직을 구성하는 프로그래밍 방법이다.

### 등장 배경
기존의 **절차 지향 프로그래밍**은 **프로세스가 함수 단위로 진행**되어 프로젝트 규모가 커지면 유지보수가 어렵고, 프로그램 분석이 어렵다는 단점이 있다. (성능을 어느 정도 포기하더라도) 이를 좀 더 빠르게 코딩하고 효율적으로 유지관리하자는 취지에서 등장한 개념으로, 객체의 관점에서 프로그래밍 하는 것을 뜻한다.객체들의 유기적인 관계를 통해 프로세스가 진행된다. 즉, **어플리케이션을 객체 단위로 구성하고 유기적으로 연결하여 프로그래밍한다.**

> ### 👀 주의
> 흔히 **절차지향 = C언어**라고 생각하는데, C언어는 문법상 OOP를 지원하지 않을 뿐, **C언어를 이용하여 객체 지향적인 구현이 불가능한 것은 아니다.** 실제로 임베디드나 펌웨어 관련 프로젝트에서는 C언어를 이용하여 큰 규모의 소스 코드를 작성해야 할 경우가 있는데, 이 때 객체 지향적인 설계를 도입하는 경우가 많다. 다만 이러한 **특수한 경우를 제외하면 C언어로 OOP를 구현하는 것은 매우 비효율적이므로 피해야 한다.**

<br />

### 클래스, 객체, 인스턴스

####  클래스 (class)

- 객체를 만들어 내기 위한 설계도 혹은 틀
- 어떤 문제를 해결하기 위한 데이터를 만들기 위해 추상화를 거쳐 집단에 속하는 속성(attribute)와 행위(behavior)를 변수와 메서드로 정의한 것

#### 객체 (object)

- 소프트웨어 세계에 구현할 대상
- 물리적으로 존재하거나 추상적으로 생각할 수 있는 것 중, 자신의 속성을 가지며 다른 객체와 식별가능한 것
- 고유한 속성과 함수로 구성된 클래스를 통해 생성된다
- 객체는 모든 인스턴스를 대표하는 포괄적인 의미이다

#### 인스턴스 (instance)

- 설계도를 바탕으로 소프트웨어 세계에 구현된 구체적인 실체
- 클래스에서 정의한 것을 토대로 실제 메모리상에 할당된 것으로 실제 프로그램에서 사용되는 데이터

<br />

### 객체 지향 특징

1. 추상화 (Abstraction)  
    필요하지 않은 정보를 제외하고 필요로 하는 속성이나 행동을 추출하는 작업
    구체적인 사물들의 공통적인 특징을 파악해서 이를 하나의 개념 혹은 집합으로 다루는 수단
2. 캡슐화 (Encaptulation)  
    객체의 속성(data fields)와 행위(methods)를 하나의 클래스라는 캡슐에 묶는 것
    외부에서 객체의 내부데이터로 직접 접근하지 못하게 통제하여 정보 은닉
3. 상속 (Inheritance)  
    하나의 클래스가 가진 특징(함수, 데이터)을 다른 클래스가 그대로 물려받는 것
    기능의 일부분을 변경하는 경우 자식 클래스에서 상속받아 수정 및 사용
    상속은 캡슐화 유지, 클래스의 재사용이 용이하도록 해준다
4. 다형성 (Polymorphism)  
    하나의 객체가 여러 가지 타입을 가질 수 있는 것  
    - 오버로딩 (Overloading) : 이름이 같은 메서드 또는 생성자를 매개변수의 개수나 타입을 다르게 정의하는 것
    - 오버라이딩 (Overriding) : 부모 클래스의 메서드와 같은 이름, 매개변수를 재정의 하는 것

<br>

### 장점과 단점
#### 장점
- 코드가 간결해진다
- 객체는 계속 활용할 수 있기 때문에 코드의 재사용성이 높아진다
- 객체 단위로 작성되면 기능에 대한 오류가 발생했을 때 개발자는 디버깅이 쉽고 유지보수가 용이하다
- 사용자는 내부적으로 데이터가 어떻게 동작하는지 몰라도 기능을 사용할 수 있기 때문에 생산성이 높아진다.
- 강한 응집력과 약한 결합력을 통해 소프트웨어의 질을 향상시킨다

#### 단점
- 처리 시간이 비교적 오래 걸린다
- 프로그램을 설계할 때 많은 고민과 시간을 투자해야 한다


## Garbage Collection

Java에선 개발자가 프로그램 코드로 메모리를 명시적으로 해제하지 않는다. 
JVM(Java Virtual Machine)이 구성된 JRE(Java Runtime Environment)가 제공되며, 그 구성 요소 중 하나인 Garbage Collection(이하 GC)이 자동으로 사용하지 않는 객체를 메모리에서 삭제하는 작업을 수행한다.

기본적으로 JVM의 메모리는 총 5가지 영역(class, stack, heap, native method, PC)으로 나뉘는데, GC는 힙 메모리만 다룬다.

### Generational Garbage Collection
Java application에서는 가비지 컬렉션을 통해 더이상 사용되지 않는 오브젝트들을 제거한다. 가비지 컬렉션에서 '더이상 사용되지 않는'의 의미는 'Unreachable object'라고 말할 수 있다. Unrechable object들은 Stack에 도달할 수 없는 Heap영역의 객체단위라고 할수 있다.

- 객체가 NULL인 경우
- 객체가 블럭 안에서 생성되고 블럭이 종료되었을 경우
- 부모 객체가 NULL이 되었을 경우, 자식 또는 포함된 객체들

### GC의 과정
deallocating memory과정은 가비지 컬렉터에 의해 자동으로 실행된다.

- Step 1: Marking
	- 가비지 컬렉터는 메모리에서 live object를 확인 하고, unrechable object가 무엇인지 마킹하는 절차를 진행한다.

- Step 2: Normal Deletion
	- 가비지 컬렉터는 unrechable object를 삭제한다.

- Step 2a: Deletion with Compacting
	- 가비지 컬렉터중 일부는 memory를 더욱 효과적으로 쓰기위해  unreachable object를 삭제함과 동시에 압축을 진행하기도 한다.

### Weak Generational Hypothesis
모든 객체를 Mark & Compact하는 방식의 Garbage Collection은 규모가 큰 프로그램에서 심각한 문제가 생길 수 있다. JVM GC 설계자들은 경험적으로 대부분의 객체가 생겨나자마자 쓰레기가 된다는 것을 알고 있었다.

Weak Generational Hypothesis는 신규로 생성한 객체의 대부분은 금방 사용하지 않는 상태가 되고, 오래된 객체에서 신규 객체로의 참조는 매우 적게 존재한다는 가설이다. 이 가설에 기반하여 Java는 Young 영역과 Old 영역으로 메모리를 분할하고, 신규로 생성되는 객체는 Young 영역에 보관하고, 오랫동안 살아남은 객체는 Old 영역에 보관한다.

1. Young Generation 영역 **`Heap영역`**
    - Eden/Survivor0/Survivor1 3개의 영역으로 구분된다. 이때, Survivor영역 중 하나는 항상 비어있다.
    - 새로 생성된 객체는 Eden영역에 할당된다.
    - 많은 객체가 Young영역에 생겼다가 Unreachable상태가 되어 사라진다. Young영역에서 객체가 사라질 때 **Minor GC가 발생**한다고 말한다.
    - Eden영역에서 GC가 한 번 발생한 후 살아남은 객체는 Survivor영역 중 하나로 이동한다. 
    - Minor GC는 Survivor영역도 같이 검사해서 살아남은 객체는 다른 Survivor영역으로 이동시킨다. 기존의 영역은 빈 상태가 된다.
    - 위의 과정을 반복하며 계속 살아남은 객체는 Old영역으로 이동한다.
2. Old Generation 영역 **`Heap영역`**
    - 대부분 Young영역보다 크게 할당된다.
    - Old영역에서 발생하는 Minor GC는 Young영역보다 GC가 적게 발생하고 시간이 오래 걸린다.
    - Unreachable상태가 되지 않고 Young영역에서 오래 살아남은 객체가 여기로 복사된다.
    - **Minor GC** : Old영역이 가득 차면 Old영역의 모든 객체들을 검사하여 참조되지 않은 객체들을 한꺼번에 삭제한다. 이때, GC를 실행하는 스레드를 제외한 모든 스레드는 작업을 멈추는데 이를 **'stop-the-world'** 라고 한다.
    > 'stop-the-world'시간을 줄이는 것이 GC튜닝!
3. Metaspace 영역
    - Java8부터 Permanent영역이 Metaspace 영역으로 변경되었다.
    - Metaspace 영역은 Native메모리 영역으로, JVM이 아닌 OS에 의해 관리되도록 변경되었다. (주로 메타데이터)
    - **Static Object만** **`Heap영역`** 으로 옮겨졌고져서 최대한 GC의 대상이 될 수 있게 하였다.
    > 참조 [JDK 8에서 Perm 영역은 왜 삭제됐을까](https://johngrib.github.io/wiki/java8-why-permgen-removed/)

#### Generational Garbage Collection 과정

1. 새로운 객체가 들어오면 Eden space에 할당된다. 두 개의 Survivor space는 비워진 상태로 시작한다.

   ![1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F2GSeE%2Fbtq2cofF3IA%2FZKPpBxMgxFhgEzOx6cazH1%2Fimg.png)

2. Eden space가 가득 차게 되면, Minor Garbage Collection이 시작된다.

   ![2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdKcHSO%2Fbtq2gJW8sKm%2Fumn7ZwEJHwgLH2JkXmdxw1%2Fimg.png)

3. 참조되는(reachable) 객체들은 첫 번째 survivor(S0)로 이동하고, 비 참조(unreachable) 객체는 Eden space가 clear 될 때 함께 메모리에서 사라진다.

   ![3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKuJ8l%2Fbtq2cYH0tcu%2FTfxIieMdjFNgKZpDSUyUG1%2Fimg.png)

4. 다음 Minor GC 때, Eden space에서 3번과 같은 과정이 발생한다. 비 참조 객체들은 지워지고, 참조 객체들은 Survivor space로 이동한다. 기존에 S0에 있던 참조 객체들은 S1으로 옮겨지는데, 이때 age 값이 증가되어 옮겨진다. 살아남은 모든 객체들이 S1으로 모두 옮겨지면 S0과 Eden space는 clear 된다.

   ![4](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0sDj7%2Fbtq2fejbE9F%2FFbQo1kaQoZ2N9TRAUOiu4k%2Fimg.png)

5. 다음 Minor GC가 발생하면 4번 과정이 반복되는데, S1이 가득 차 있었으므로 S1에서 살아남은 객체들은 S0으로 옮겨지면서 Eden과 S1은 clear 된다. 이때도 age 값이 증가되어 옮겨진다.

   > 💡 Survivor 영역 중 하나는 반드시 비어 있는 상태로 남아 있어야 한다. 만약 두 Survivor 영역에 모두 데이터가 존재하거나, 두 영역 모두 사용량이 0이라면 시스템이 정상적인 상황이 아니라고 생각하면 된다.

   ![5](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdirtUA%2Fbtq2ggVv0Fj%2FmUfT9KJYtUPK0DQTWkCV8k%2Fimg.png)

6. Young Generation에서 계속해서 살아남으며 age 값이 증가하는 객체들은 age 값이 특정값 이상이 되면 Old Generation(Java 8까지는 Tenured Generation이라 부름)으로 옮겨지는데 이 단계를 Promotion(진급)이라고 한다.

   ![6](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnOTRT%2Fbtq2hZx23Zv%2FzsS5VLmSmDJL3FGmRNM1Ok%2Fimg.png)

7. Minor GC가 계속해서 반복된다면, Promotion 작업도 꾸준히 발생하게 된다.

   ![7](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbORCfm%2Fbtq2hIwtsMW%2FaScgxO9wIKfd7ba2Kzjua0%2Fimg.png)

8. Promotion 작업이 게속해서 반복되면서 Old Generation이 가득 차게 되면 Major GC가 발생하게 된다. 그렇게 Old Generation이 clear 되고 공간이 compact 된다.

   ![8](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJEPsg%2Fbtq2h3AnBTz%2FeGo8NBLU31DqAQTxmycKg1%2Fimg.png)


[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#java)

<br>

## SOLID
객체 지향의 4대 특성인 캡슐화, 상속, 추상화, 다형성을 이용하여 객체 지향을 올바르게 설계할 수 있도록 도와주는 원칙들이다. SOLID는 클래스 안의 응집도는 높이고 타 클래스들 간의 결합도는 낮추는 High Cohesion - Loose Coupling 원칙을 객체 지향의 관점에서 도입한 것이다.

### SRP (Single Responsibility Principle) 단일 책임 원칙
> 객체는 단하나의 책임만 가져야 한다는 원칙

- 클래스에 모든 기능을 다넣기 보다는 목적과 취지에 맞도록 관련된 책임만 주도록 하는것

- 즉, 속성 메서드 모듈, 컴포넌트 , 프레임워크를 단일책임으로 주고 독립적으로 모듈화 시키는 것이 SRP(단일책임) 원칙이다.

|                         SRP 적용 전                          |                         SRP 적용 후                          |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://sehun-kim.github.io/sehun/assets/images/1541862421739.png"> | <img src="https://sehun-kim.github.io/sehun/assets/images/1541913074849.png"> |

<br>

### OCP (Open Closed Principle) 개방 폐쇄 원칙
> 기존의 코드를 변경하지 않고 기능을 추가할수 있도록 설계해야 하는 원칙

- 소프트웨어의 구성요소(컴포넌트, 클래스, 모듈, 함수)는 확장에 대해 열려있지만, 변경에는 닫혀 있어야 한다.

- 자신의 확장에서는 개방되어있고, 주변의 변화에 대해서는 패쇄되어 있어야한다.

- 즉, 구현된 클래스를 이용하기 보다는 Interface 나 abstract class 를 만들어 완충제 역할을 하도록한다.



|                         OCP 적용 전                          |                         OCP 적용 후                          |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://sehun-kim.github.io/sehun/assets/images/1541913936787.png"><br /><img src="https://sehun-kim.github.io/sehun/assets/images/1541913944420.png"><br />운전자는 마티즈와 쏘나타의 변화에 따라 행동이 의존적으로 변하게 된다. 이와 같이 직접적으로 클래스의 메서드를 호출하고 결합도를 높게 설계했다면 확장적이지 못할 뿐더러 많은 수정이 발생되어 유지 보수가 어려워진다. | <img src="https://sehun-kim.github.io/sehun/assets/images/1541913950879.png"><br />마티즈, 쏘나타의 상위에 자동차라는 상위 클래스를 둔다. 이렇게 되면 자동차 클래스는 하위에 다른 차종을 상속하여 확장할 수 있고, 운전자는 그 변경 사항에 전혀 영향을 받지 않을 수 있다. |

<br>

### LSP (Liskov Substitution Principle) 리스코프 치환 원칙
> 자식 클래스는 최소한 부모 클래스의 기능은 수행할수있어야 한다는 원칙

- 서브 타입은 언제나 자신의 상위타입으로 교체할 수 있어야한다.

- 상위 객체의 타입을 하위 객체의 타입으로 변경했을 때, 작동에 이상이 없어야 한다.


|                        LSP 위반 사례                         |                     LSP를 잘 구현한 사례                     |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://sehun-kim.github.io/sehun/assets/images/1541915202328.png"><br />딸은 아버지의 역할을 할 수 없다. | <img src="https://sehun-kim.github.io/sehun/assets/images/1541915209305.png"><br />박쥐는 포유류의 역할을 할 수 있다. |

<br>

### ISP (Interface Segregation Principle) 인터페이스 분리 원칙
> 자신이 사용하지 않는 인터페이스와 의존관계를 맺거나 영향을 받지 않아야 한다는 원칙

- 클라이언트는 자신과 관련이 없는 메서드에 의존하지 않아야 한다.  
- SRP 로 인해 너무 많은 클래스 구현을 불러일으킬때, 큰 덩어리의 인터페이스들을 역할 인터페이스(구체적이고 작은 단위)로 분리시켜서 해당 역할만 구현하도록 한다.

|                             SRP                              |                             ISP                              |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://sehun-kim.github.io/sehun/assets/images/1541913074849.png"><br />역할마다 클래스를 분리한다. | <img src="https://sehun-kim.github.io/sehun/assets/images/1541915738069.png"><br />다양한 역할을 인터페이스로 만들고 각 역할에 맞는 메소드만 제공한다. |

<br>

### DIP (Dependency Inversion Principle) 의존 역전 원칙
> 의존관계 성립시 추상성이 높은 클래스와 의존 관계를 맺어야 하는 원칙

- 하위클래스나 구체클래스가 아닌 더 추상적인 것에 의존하라는 것이 의존 역전 원칙이다.
- 만약 DIP 가 지켜지지 않는 다면 클래스가 수정 될떄마다 상위클래스들이 수정 되어야한다.
- 따라서 자신보다 변하기 쉬운 것에 의존하던것을 인터페이스나 상위클래스를 두어 하위 클래스의 변화에 영향을 받지 않도록 설계한다.

|                         DIP 적용 전                          |                         DIP 적용 후                          |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://sehun-kim.github.io/sehun/assets/images/1541916908016.png"><br />자동차는 스노우타이어에 의존하고 있다. | <img src="https://sehun-kim.github.io/sehun/assets/images/1541917299687.png"><br />해당 의존 관계를 타이어 인터페이스를 사용하여 역전시킨다.<br />즉, 구체적인 스노우타이어에 의존하던 것을 추상적인 타이어에 의존하는 것으로 변경했다. |
<br>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#java)
<br>

<br />

## Serialization (직렬화)

```text
Java 내부 시스템에서 사용되는(또는 JVM 메모리에 상주(힙or스택)되어 있는) 객체나 데이터를 외부에서 사용할 수 있도록 Byte 형태로 변환하는 것. 
```

![직렬화](https://i0.wp.com/techvidvan.com/tutorials/wp-content/uploads/sites/2/2020/05/Serialization-and-Deserialization-in-Java.jpg?w=802&ssl=1)

> 🎯 목적 : 객체를 상태 그대로 저장하고, 필요할 때 다시 생성하여 사용하는 것

대부분의 OS의 프로세스 구현은 서로 다른 가상 메모리 주소공간을 갖기 때문에 Reference Type의 데이터들은 인스턴스를 전달할 수 없다. 서로 다른 메모리 공간 사이의 데이터 전달을 위해서 메모리 공간의 주소값이 아닌 Byte 형태로 직렬화된 객체 데이터를 전달하면, 사용하는 쪽에서 역직렬화하여 사용할 수 있게 된다.

직렬화된 데이터는 모두 Primitive Type (Byte Type)의 데이터 묶음이며, 이것이 파일 저장이나 네트워크 전송 시 파싱할 수 있는 유의미한 데이터가 된다. 즉, 전송/저장 가능한 데이터를 만드는 것이 `직렬화(Serialization)`이다. 

JSON, CSV 등의 포맷은 직렬화/역직렬화 시에 특정 라이브러리를 도입해야 쉽게 개발이 가능하며, 구조가 복잡해지면 직접 매핑해줘야 하지만, Java 직렬화는 비교적 복잡한 객체도 큰 작업 없이 `java.io.Serializable` 인터페이스만 구현해주면 기본 Java 라이브러리만 사용해도 직렬화/역직렬화가 가능하다.

<br />

### 직렬화 조건 및 방법

- `java.io.Serializable` 인터페이스를 상속받은 객체
  - 객체의 크기는 가변적이며, 객체를 구성하는 자료형들의 종류와 수에 따라 객체의 크기가 다양하게 바뀔 수 있기 때문에 객체를 직렬화하기 위해 `Serializable` 인터페이스 구현
- Primitive 타입의 데이터
  - 정해진 Byte 변수이기 때문에 Byte 단위로 변환하는 것에 문제가 없다.
- 객체의 멤버들 중 Serializable 인터페이스가 구현되지 않은 것이 존재하면 안된다.
- `transient`가 선언된 멤버는 전송되지 않는다.
  - 객체 내에 Serializable 인터페이스가 구현되지 않은 멤버는 `transient`를 선언해주면 NonSerializableException이 발생하지 않는다.
- `java.io.ObjectOutputStream`을 이용하여 직렬화한다.

<br />

### 직렬화가 사용되는 곳

- JVM의 메모리에서 상주하는 객체 데이터를 그대로 영속화(Persistence)할 때 사용된다.
  - 시스템이 종료되더라도 사라지지 않으며, 영속화된 데이터이기 때문에 네트워크로 전송도 가능하다.

- Servlet Session
  - Servlet 기반의 WAS들은 대부분 세션의 Java 직렬화를 지원한다.
  - 세션을 서블릿 메모리 위에서 운용한다면 직렬화를 필요로 하지 않지만, 파일로 저장하거나 세션 클러스터링, DB를 저장하는 옵션 등을 선택하게 되면 세션 자체가 직렬화되어 저장되어 전달된다.

- Cache
  - 캐시할 부분을 직렬화된 데이터를 저장해서 사용한다.
  - 실시간으로 요구되는 데이터가 아니라면 저장소를 이용해서 데이터 객체를 저장한 후 동일한 요청이 오면 DB를 다시 요청하는 것이 아니라 저장된 객체를 찾아서 응답하게 한다.
  - 이때 저장된 객체를 구성할 때 직렬화를 이용하면 간편하게 때문에 많이 사용한다.
  - Ehcache, Redis, Memcached 라이브러리 시스템에서 많이 사용된다.

- Java RMI(Remote Method Invocation)
  - 원격 시스템 간의 메세지 교환을 위해서 자바에서 지원하는 기술
  - 원격 시스템의 메서드를 호출할 때 전달하는 메세지(객체)를 직렬화하여 사용
  - 메세지(객체)를 전달받은 원격 시스템에서는 메세지(객체)를 역직렬화하여 사용

- 객체가 세션에 저장하지 않는 단순한 데이터 집합이고, 컨트롤러에서 생성되어서 뷰에서 소멸하는 데이터의 전달체라면 객체 직렬화는 고려하지 않아도 된다.

- 세션 관리를 스토리지나 네트워크 자원을 사용한다면 객체 직렬화를 해야 하고, 메모리에서만 관리한다면 객체 직렬화를 할 필요가 없다. 둘 다 고려한다면 직렬화가 필요하다.

- 직렬화는 Java 시스템 개발에 최적화되어 있지만 데이터의 만료시점이나 구조, 필요시점 등 여러가지를 고려하여 CSV, JSON 등과 적절히 판단하여 사용하면 된다.

<br />

### 장점과 단점

장점

- 복잡한 데이터 구조의 클래스라도 기본 조건만 지키면 바로 직렬화가 가능하다.
- 데이터 타입이 자동으로 맞춰진다.

단점

- 직렬화된 데이터의 사이즈가 매우 크다.
  - json 등의 경량화된 형태로 저장하는 것이 좋을 수 있다.
- 역직렬화시 예외가 생길 수 있음을 주의해야 한다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#java)

<br />

### Deserialization (역직렬화)

```text
Byte로 변환된 데이터를 원래대로 객체 또는 데이터로 변환하는 기술.
직렬화된 바이트 형태의 데이터를 객체로 변환하여 JVM 내에 상주시킨다.
```

#### 역직렬화 조건 및 방법

- `java.io.Serializable` 인터페이스를 구현한 클래스의 객체여야 한다.
- 해당 객체의 클래스가 class path에 존재해야 하며, import 되어 있어야 한다.
- 직렬화, 역직렬화를 진행하는 시스템이 서로 다를 수 있다는 점을 고려해야 한다.
- Java 직렬화 대상 객체는 동일한 `serialVersionUID`를 가지고 있어야 한다.
- `java.io.ObjectInputStream`을 사용하여 역직렬화를 진행한다.

<br />

### SerialVersionUID

직렬화된 객체를 역직렬화 할 때는 직렬화 할 때의 클래스와 같은 클래스를 사용해야 되는데, 클래스 명이 같더라도 내용이 변경되면 역직렬화에 실패한다. 따라서, `serialVersionUID`를 통해 클래스 일치 여부를 확인한다. 

명시적으로 선언하지 않으면 해시값이 생성되어 할당되어 직렬화 내용에 포함된다. 하지만 기존의 클래스 멤버 변수가 변경되면 `serialVersionUID`가 달라지는데, 역직렬화 시 달라진 넘버로 Exception이 발생할 수 있다. 따라서 직접 `serialVersionUID`를 관리해야 클래스의 변수가 변경되어도 직렬화에 문제가 발생하지 않게 된다.

> `serialVersionUID`을 관리하더라도, 멤버 변수의 타입이 다르거나, 제거 혹은 변수명을 바꾸게 되면 Exception은 발생하지 않지만 데이터가 누락될 수 있다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#java)

<br>

## JVM 작동방식 및 Java의 메모리 영역

### JVM (Java Virtual Machine)

- 스택 기반의 가상머신
- 자바 애플리케이션을 클래스 로더를 통해 읽어들여 자바 API와 함께 실행시킨다.
- Java와 OS 사이에서 중개자 역할을 수행하여 **Java가 OS에 구애 받지 않고 실행**할 수 있게 해준다.
- 모든 기본 타입의 정의를 명확히 함으로써 플랫폼 독립성 보장
- 메모리 관리, Garbage Collection 수행
- 자바 프로그램을 실행시킨다 == JVM을 실행시키고 그 위에서 자바 프로그램을 실행시킨다

#### Java 프로그램 실행 과정
![자바 프로그램 실행 단계](https://t1.daumcdn.net/cfile/tistory/99960C505C37390616)

1. 프로그램이 실행되면 JVM은 OS로부터 이 프로그램이 필요로 하는 **메모리를 할당**받는다.
    
    JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.
    
2. **자바 컴파일러**(javac)가 자바 소스코드(.java)를 읽어들여 자바 바이트코드(.class)로 변환시킨다.
3. 컴파일 된 바이트코드(.class)는 JVM의 클래스 로더에게 전달된다.
4. **클래스 로더**는 동적로딩을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터 영역, 즉 JVM의 메모리에 올린다.
5. 실행 엔진은 JVM 메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 해석한다.
6. 해석된 바이트코드는 Runtime Data Area에 배치되어 실질적인 수행이 이루어지게 된다.

이러한 실행 과정 속에서 JVM은 필요에 따라 `Thread Synchrnoization`과 `GC` 같은 관리작업을 수행한다.

### JVM의 구성 및 동작원리

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F990BF73B5B73A02817" style width="400"/>

#### 클래스 로더

- 자바 바이트코드(.class)파일을 읽어서 바이트 코드를 `Method Area`에 저장한다.
- 런타임 시 **클래스를 처음으로 참조할 때** 해당 클래스를 로드하고 링크한다.

[클래스 로더의 특징](https://github.com/GumiMobile/CS-Study/blob/main/JAVA/Problem%20List/JVM%20%EC%9E%91%EB%8F%99%EB%B0%A9%EC%8B%9D%20%EB%B0%8F%20Java%EC%9D%98%20%EB%A9%94%EB%AA%A8%EB%A6%AC%20%EC%98%81%EC%97%AD.md#%ED%81%B4%EB%9E%98%EC%8A%A4-%EB%A1%9C%EB%8D%94)

---

#### 런타임 데이터 영역

- JVM이 OS 위에서 실행되면서 할당받는 메모리 영역
- 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역
- 5가지 영역으로 나눌 수 있다.

<img src="[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99B467465B73D15111](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99B467465B73D15111)" style width="400"/>

위 그림에서 보라색 영역은 스레드마다 생성되고, 힙, 메서드 영역은 스레드가 공유해서 사용한다.

##### PC Register (Program Counter Register)

- Thread가 시작될 때 생성되며 생성될 때마다 생성되는 공간으로 스레드마다 하나씩 존재
- Thread가 어떤 부분을 어떤 명령으로 실행해야할지에 대한 기록을 하는 부분
- 현재 수행 중인 JVM 명령의 주소를 가지고 있다.

##### JVM Stack

- 프로그램 실행 과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역
    - 각종 형태의 변수나 임시 데이터, 스레드나 메소드의 정보 저장 
    (호출된 메소드의 매개변수, 지역변수, 리턴값, 연산 시 생기는 값 등)
- Stack Frame이라는 구조체를 저장하는 스택이다. 예외 발생 시, printStackTrace() 메서드로 보여주는 Stack Trace의 라인들이 스택 프레임을 표현한다.
    - 메소드 호출 시마다 각각의 스택 프레임(그 메소드만을 위한 공간)이 생성된다.
    - 메소드 수행이 끝나면 프레임별로 삭제

##### Native Method Stack

- 자바 프로그램이 컴파일되어 생성되는 바이트코드가 아닌 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역
- Java가 아닌 다른 언어로 작성된 네이티브 코드를 위한 공간
- Java Native Interface를 통해 바이트코드로 전환하여 저장
- 일반 프로그램처럼 커널이 스택을 잡아 독자적으로 프로그램을 실행시킨다.
- JNI(Java Native Interface)를 통해 호출하는 C/C++ 등의 코드를 수행하기 위한 스택으로, 언어에 맞게 생성된다.
    - 이 부분을 통해 C 코드를 실행시켜 커널에 접근 가능

##### Method Area (= Class Area, Static Area)

- 모든 스레드가 공유하는 영역으로 JVM이 시작될 때 생성된다.
- GC의 관리 대상에 포함된다.
- 클래스 정보를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간
- 올라가게 되는 메소드의 바이트코드는 프로그램의 흐름을 구성하는 바이트코드
    - 자바 프로그램은 main 메소드의 호출에서부터 계속된 메소드의 호출로 흐름을 이어가기 때문
    - 대부분의 인스턴스 생성도 메소드 내에서 명령하고 호출한다.
    - 사실상 컴파일된 바이트코드의 대부분이 메소드 바이트코드이기 때문에 거의 모든 바이트코드가 올라간다고 봐도 상관없다.
- 올라가는 정보의 종류
    - Runtime Constant Pool (별도의 관리 영역)
        
        > 런타임 상수 풀(Runtime constant pool)
        > 
        > 
        > JVM 동작에서 가장 핵심적인 역할을 수행하는 곳으로 중요하다. 각 클래스와 인터페이스의 상수 뿐만 아니라, 메서드와 필드에 대한 모든 레퍼런스까지 갖고 있는 테이블로, 어떤 메서드나 필드를 참조할 때, JVM은 런타임 상수 풀을 통해 해당 메서드나 필드의 실제 메모리 주소를 찾는다.
        > 
        - 상수 자료형을 저장하여 참조하고 중복을 막는 역할 수행
    - Field Information
        - 멤버변수의 이름, 데이터 타입, 접근 제어자에 대한 정보
    - Method Information
        - 메소드의 이름, 리턴타입, 매개변수, 접근 제어자에 대한 정보
    - Type Information
        - class인지 interface인지 여부 저장/Type 속성, 전체 이름, super class의 전체 이름(interface이거나 object인 경우 제외)
    - Class Variable
        - static 키워드로 선언된 변수인 클래스 변수가 저장된다.
        

##### Heap

아래 그림은 Java 8이전까지의 Heap 메모리 구조이다.

![Heap](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKA8I0%2Fbtq42yTNQ0q%2F855nAKHEBJP8fFLkPAzjS0%2Fimg.png)

- 인스턴스, 객체를 저장하는 가상 메모리 공간으로 GC의 대상이다.
- JVM 성능 이슈에서 가장 많이 언급되는 공간이다. 힙 구성, GC 방법은 JVM 벤더들의 소관이다.
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
        

#### 실행 엔진

클래스 로더를 통해 런타임 데이터 영역에 배치된 바이트 코드를 명령어 단위로 읽어 실행한다. 바이트 코드의 각 명령어는 1바이트 크기의 Operation Code와 추가 피연산자로 이뤄져 있다. Operation Code를 기계가 실행할 수 있는 형태로 변환하는데 2가지 방법이 있다.

- 인터프리터
    
    바이트 코드 명령어를 하나씩 읽고 해석하여 실행한다. 명령어 하나의 해석은 빠르나 전체 실행 속도는 느리다. JVM안에서 바이트 코드는 일반적으로 인터프리터 형태로 실행된다.
    
- JIT 컴파일러 (Just In time Compiler)
    
    인터프리터의 단점을 보완하는 기술로, 바이트 코드 전체를 컴파일하여 네이티브 코드로 변경한다. 변경 후, 인터프리팅 하지 않고 네이티브 코드로 직접 실행한다. 변환된 네이티브 코드는 캐시에 보관하기 때문에 실행속도는 빠르지만 변환 과정이  인터프리팅 하는 것보다 오래 걸린다. 따라서 사용 횟수 기준을 넘은 메서드만 JIT 컴파일러를 통해 변환한다. 변환과정은 IR(Intermediate Representaion)을 거친 후, 최적화를 수행하고 네이티브 코드로 변환된다.
    
    > Intermediate Representaion
    > 
    > - 소스 코드를 표현하기 위해 컴파일러 또는 가상 시스템에서 내부적으로 사용하는 데이터 구조 또는 코드이다.
    > - IR은 최적화와 인터프리팅 등 추가 처리를 위해 도움이 되도록 설계되었다.
    > - 정보 손실 없이 소스 코드를 표현 가능하며, 특정 소스, 언어와 무관하게 코드를 나타낼 수 있다.

#### Garbage Collector

Garbage Collector(GC)는 Heap 메모리 영역에 생성(적재)된 객체들 중에 참조되지 않는 객체들을 탐색 후 제거하는 역할을 한다.

GC가 역할을 하는 시간은 정확히 언제인지를 알 수 없다. (참조가 없어지자마자 해제되는 것을 보장하지 않음)

또 다른 특징은 GC가 수행되는 동안 GC를 수행하는 쓰레드가 아닌 다른 모든 쓰레드가 일시정지된다.

특히 Full GC가 일어나서 수 초간 모든 쓰레드가 정지한다면 장애로 이어지는 치명적인 문제가 생길 수 있는 것이다. (GC와 관련된 내용은 아래 Heap영역 메모리를 설명할 때 더 자세히 알아본다.)

### (참고) 메모리 누수 현상

컴퓨터 프로그램이 필요하지 않은 메모리를 계속 점유하고 있는 현상.
메모리 동적 할당 시 Heap에 할당되는데, 사용자가 해제하지 않아 Heap메모리 공간을 계속 차지하면 나중에 메모리가 부족하여 시스템이 다운될 수 있다.


[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#java)

<br>
