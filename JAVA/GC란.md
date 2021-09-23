# GC 란?

## 이유진

### Garbage Collection

Java에선 개발자가 프로그램 코드로 메모리를 명시적으로 해제하지 않는다. 따라서 가비지 컬렉터가 더 이상 필요 없는 객체(쓰레기)를 찾아 메모리에서 제거한다. 

> **GC의 두 가지 전제조건**
> 1. 대부분의 객체는 금방 접근 불가능 상태(unreachable)가 된다.
> 2. 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.

### GC 과정
|영역별 데이터 흐름|Java JVM|
|--|--|
|![image](https://user-images.githubusercontent.com/37680108/134037893-d2f23af3-4800-4600-9422-d5a1e5fc3f6e.png)|<img width="500" src="https://user-images.githubusercontent.com/37680108/134043245-ff4f6cdf-5cdd-49b6-be7c-d6426dfc7732.png">  |
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

### 우지현

#### Garbage Collection

C/C++ 언어와 달리 Java는 개발자가 명시적으로 객체를 해제할 필요가 없다. Java에서는 JVM(Java Virtual Machine)이 구성된 JRE(Java Runtime Environment)가 제공되며, 그 구성 요소 중 하나인 Garbage Collection(이하 GC)이 자동으로 사용하지 않는 객체를 메모리에서 삭제하는 작업을 수행한다.  

기본적으로 JVM의 메모리는 총 5가지 영역(class, stack, heap, native method, PC)으로 나뉘는데, GC는 힙 메모리만 다룬다. 일반적으로 다음과 같은 경우에 GC의 대상이 된다.

- 객체가 NULL인 경우
- 객체가 블럭 안에서 생성되고 블럭이 종료되었을 경우
- 부모 객체가 NULL이 되었을 경우, 자식 또는 포함된 객체들

#### GC의 메모리 해제 과정

1. Marking

   ![Marking](https://github.com/GimunLee/tech-refrigerator/raw/master/Language/JAVA/resources/java-gc-001.png)

   프로세스는 마킹을 호출하여 메모리가 사용되는지 아닌지를 찾아낸다. 참조되는 객체는 파란색으로, 참조되지 않는 객체를 주황색으로 보여진다. 마킹 단계에서 모든 오브젝트가 스캔되기 때문에 매우 많은 시간이 소모된다.

2. Normal Deletion

   ![Normal Deletion](https://github.com/GimunLee/tech-refrigerator/raw/master/Language/JAVA/resources/java-gc-002.png)

   참조되지 않는 객체를 제거하고 메모리를 반환한다. Memory Allocator는 반환되어 비어진 블럭의 참조 위치를 저장해 두었다가 새로운 오브젝트가 선언되면 할당되도록 한다.

3. Compacting

   ![Compacting](https://github.com/GimunLee/tech-refrigerator/raw/master/Language/JAVA/resources/java-gc-003.png)

   퍼포먼스를 향상시키기 위해, 참조되지 않는 객체를 제거하고 남은 객체들을 묶는다. 이들을 묶음으로써 공간이 생기므로 새로운 메모리를 할당할 때 더 쉽고 빠르게 진행할 수 있다.

#### Weak Generational Hypothesis

모든 객체를 Mark & Compact하는 방식의 Garbage Collection은 규모가 큰 프로그램에서 심각한 문제가 생길 수 있다. JVM GC 설계자들은 경험적으로 대부분의 객체가 생겨나자마자 쓰레기가 된다는 것을 알고 있었다. 

Weak Generational Hypothesis는 신규로 생성한 객체의 대부분은 금방 사용하지 않는 상태가 되고, 오래된 객체에서 신규 객체로의 참조는 매우 적게 존재한다는 가설이다. 이 가설에 기반하여 Java는 Young 영역과 Old 영역으로 메모리를 분할하고, 신규로 생성되는 객체는 Young 영역에 보관하고, 오랫동안 살아남은 객체는 Old 영역에 보관한다.

#### Generational Garbage Collection

![Generation Garbage Collection](https://github.com/GimunLee/tech-refrigerator/raw/master/Language/JAVA/resources/java-gc-006.png)

1. Young 영역 (Young Generation 영역)

   새롭게 생성한 객체의 대부분이 Young 영역에 위치한다. 대부분의 객체가 금방 접근 불가능 상태가 되기 때문에 매우 많은 객체가 Young 영역에 생성되었다가 사라진다. 이 영역에서 객체가 사라질 때 Minor GC가 발생한다고 한다.

2. Old 영역 (Old Generation 영역)

   접근 불가능 상태로 되지 않아 Young 영역에서 살아남은 객체가 Old 영역으로 복사된다. 대부분 Young 영역보다 크게 할당하며, 크기가 큰 만큼 Young 영역보다 GC는 적게 발생한다. 이 영역에서 객체가 사라질 때 Major GC(of Full GC)가 발생한다고 한다.

3. Permanent 영역

   Method Area라고도 한다. JVM이 클래스들과 메서드들을 설명하기 위해 필요한 메타 데이터들을 포함하고 있다. JDK8부터는 PermGen은 Metaspace로 교체된다.

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

### 이수형

Garbage Collection

- 메모리를 비워주는 동작

Garbage Collector

- Garbage Collection을 동작하는 알고리즘 


### Garbage Collection의 영역

Young

- 생성된지 얼마 안된 객체들이 저장되는 장소이고 Eden 영역과 두개의 서바이버 영역으로 나뉘어 있음
- Minor GC가 일어남

Old 

- 오랫동안 사용중인 객체들이 저장되는 장소
- Full GC가 일어남

Metaspace 

- 클래스, 메타데이터가 저장되는 곳

### Minor GC

작동방식은 Eden영역이 가득차게되면 참조되지 않는 객체들은 비우고 남은 객체들을 서바이버 영역으로 옮긺 (이때 한쪽의 서바이버 영역은 비워져 있어야함 )
그리고 서바이버 영역으로 옮길 때마다 기존에 저장되있는 객체들의 age값이 늘어나며 기준 age값을 넘어가면 Old 영역으로 올라감(promotion 과정)

### Full GC

 Old 영역도 Young 영역처럼 메모리가 가득차면 똑같은 GC가 일어남
Minor GC보다 시간이 오래걸림


## 윤상일

### 💡 GC란?

**가비지 컬렉션(Garbage Collection)**, JVM 기반 언어(Java, Kotlin)에서 유효하지 않은 메모리를 알아서 정리해주는 시스템을 의미해요

객체는 대부분 일회성이므로, GC에서는 객체의 생존 기간에 따라 물리적인 Heap 영역을 나눠서 사용해요. 바로 Young 영역과 Old영역이에요



### 💡 Young 영역(Young Generation)

- 새롭게 생성된 객체가 할당되는 영역이에요
- 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다가 사라져요
- Young 영역에 대한 GC를 **Minor GC**라고 불러요
- 실행 속도가 빨라요



### 💡 Old 영역(Old Generation)

- Young 영역에서 Reachable 상태를 오래 유지하여 살아남은 객체가 복사되는 영역이에요
- 복사되면서 대부분 Young 영역보다 크게 할당되며, 크기가 큰 만큼 Garbage는 적게 발생해요
- Old 영역에 있는 객체가 Young 영역의 객체를 참조하는 경우를 대비하여 Old 영역 내에 512bytes의 덩어리(Chunk)로 되어 있는 카드 테이블(Card Table)이 존재해요
- Old 영역에 대한 GC를 **Major GC** 또는 **Full GC**라고 불러요
- 실행 속도가 느려요

## 김현수

### GC(Garbage Collector)
JVM 기반 언어와 C, C++과 가장 큰 차이점은 free()와 같은 메모리 접근을 통한 명시적 메모리 해제 여부이다. 자바는 OS메모리 영역에 직접 접근하지 않고 JVM이라는 Process이자 가상머신을 통해 접근한다. 
자바 application은 System.gc()와 같은 명시적 명령어를 통해 garbage collection을 수행할 수 있으나, 시스템의 성능에 매우 큰 영향을 미치므로 권장되지 않는다. 대신 JVM의 옵션에 따라 Serial GC, Parallel GC, CMS(Concurrent Mark Sweep) GC, G1 GC(after java 7)가 사용되고, 각 GC들은 특정 알고리즘과 주어진 옵션에 따라 메모리 관리를 하게 된다.


### Garbage
Java application에서는 가비지 컬렉션을 통해 더이상 사용되지 않는 오브젝트들을 제거한다. 가비지 컬렉션에서 '더이상 사용되지 않는'의 의미는 'Unreachable object'라고 말할 수 있다. Unrechable object들은 Stack에 도달할 수 없는 Heap영역의 객체단위라고 할수 있다.

### Garbage Collections 과정

deallocating memory과정은 가비지 컬렉터에 의해 자동으로 실행된다.

- Step 1: Marking
	- 가비지 컬렉터는 메모리에서 live object를 확인 하고, unrechable object가 무엇인지 마킹하는 절차를 진행한다.

- Step 2: Normal Deletion
	- 가비지 컬렉터는 unrechable object를 삭제한다.

> - Step 2a: Deletion with Compacting
>	- 가비지 컬렉터중 일부는 memory를 더욱 효과적으로 쓰기위해  unreachable object를 삭제함과 동시에 압축을 진행하기도 한다.

### GC 알고리즘
GC 알고리즘의 기본 흐름은 GC 대상을 식별하고, 식별된 대상을 메모리에 제거하며, 필요한 경우 최적화까지 수행한다. 간단해 보이지만 이러한 알고리즘에도 여러 종류가 있으며, 각각 다음과 같다.

#### Reference Counting Algorithm
- Garbage의 탐색에 초점을 맞춘 초기 알고리즘이다. 
- 각 객체마다 Reference Count 라는 것을 관리하는데, 말 그대로 참조 되고 있는 갯수를 의미하며, Reference Count가 0이되면 GC를 수행한다. 
- 단순한 구조인데다가, Reference Count가 0이 되면 즉시 메모리에서 해제된다는 장점이 있다.
- Reference Count를 계속 관리해주어야 하고, Linked List 같은 순환 참조 구조에서 Memory Leak이 발생할 가능성이 크다.

#### Mark-Sweep-Compaction
- 기본적인 GC 과정으로, 다양한 GC에서 사용되는 알고리즘이다.
- 이름 그대로, GC 대상 객체를 식별(Mark)하고, 청소(Sweep)하고, 압축(Compaction, 앞에서부터 채움)한다.
- Root Set에서 시작하는 Reference의 관계를 추적하며, Tracing Algorithm이라고도 불린다.
- Mark 단계에서는 Garbage 대상 외 살아남을 객체를 마킹하며, 마킹 방식은 각 객체의 Header에 Flag를 심거나 별도의 BitmapTable을 이용한다.
- Sweep 단계는 Mark 단계가 끝나면 바로 실행되며, 마킹이 없는 객체를 모두 삭제하는 작업을 한다. Sweep이 완료되면 살아남은 모든 마킹 정보를 초기화한다.
- Compact 단계에서는 Sweep 단계 후 살아남은 객체들 사이사이의 빈 공간, 즉 단편화를 살아남은 객체들을 이어붙여 해결한다.
	- 이 작업 이후 살아남은 객체들의 Reference를 업데이트하는 작업이 필요하여 부가적인 Overhead가 수반된다.

### Java Garbage Collector의 종류

- Serial GC
- Parallel GC
- CMS(Concurrent Mark Sweep) Collector
- G1 Garbage Collector
- Z GC
