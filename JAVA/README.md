# JAVA

- [객체 지향 프로그래밍이란 무엇인가?](#객체-지향-프로그래밍이란-무엇인가)
- [Garbage Collection](#Garbage-Collection)
- SOLID](#SOLID)
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
