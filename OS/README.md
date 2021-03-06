# Operating System
* [프로세스와 쓰레드의 차이](#프로세스와-쓰레드의-차이)
* [스케줄러](#스케줄러)
* [멀티쓰레드(장점, 단점)](#멀티쓰레드장점-단점)
* [프로세스 동기화](#프로세스-동기화)
  * [프로세스 동기화란?](#프로세스-동기화란?)
  * [경쟁 조건](#경쟁-조건)
  * [스레드 동기화란?](#스레드-동기화란?)
  * [구역(Section)](#구역section)
  * [임계구역 문제](#임계구역-문제)
  * [임계구역 문제 해결책](#임계구역-문제-해결책)
* [동기와 비동기의 차이](#동기와-비동기의-차이)
  * [동기(Sync, synchronous)](#동기Sync-synchronous)
  * [비동기(Async, asynchronous)](#비동기Async-asynchronous)  
  * [동기와 비동기 예시](#동기와-비동기-예시)
  * [동기식 입출력 vs 비동기식 입출력](#동기식-입출력-vs-비동기식-입출력)
* [CPU 스케줄러](#CPU-스케줄러)
  * [CPU 스케줄링 종류](#CPU-스케줄링-종류)
* [메모리 관리 전략1 (메모리 관리 배경)](#메모리-관리-전략1-메모리-관리-배경) 
* [메모리 관리 전략2 (Paging)](#메모리-관리-전략2-Paging)
* [메모리 관리 전략3 (Segmentation)](#메모리-관리-전략3-segmentation)
  * [세그멘테이션 (Segmentation)](#세그멘테이션-segmentation)
* [가상메모리](#가상메모리)
  * [가상 메모리 (배경, 가상 메모리가 하는 일, Demand Paging)](#가상-메모리-배경-가상-메모리가-하는-일-Demand-Paging)
  * [페이지 교체 알고리즘](#페이지-교체-알고리즘)
* [메모리의 구조](#메모리의-구조)
  * [코드(code) 영역](#코드code-영역)
  * [데이터(data) 영역](#데이터data-영역)
  * [스택(stack) 영역](#스택stack-영역)
  * [힙(heap) 영역](#힙heap-영역)
  * [스택과 힙의 주소 할당 방향](#스택과-힙의-주소-할당-방향)
  * [오버플로우](#오버플로우)
* [캐시의 지역성](#캐시의-지역성-locality-caching-line)
* [교착상태](#교착상태-dead-lock)
  * [교착상태의 발생조건](#교착상태의-발생조건)
  * [교착상태의 해결법](#교착상태의-해결법)

[뒤로](https://github.com/GumiMobile/CS-Study)

<br><br>
## 프로세스와 쓰레드의 차이

## 프로세스

OS로부터 자원(CPU시간, 운영에 필요한 주소 공간, [Code,Data,Stack,Heap]의 구조로 된 독립된 메모리 영역)을 할당받는 작업의 단위

하나의 쓰레드가 기본생성되기때문에 하나이상의 쓰레드를 보유함

- Code : 프로세스의 정의, 명령
- Data : 프로그램이 사용하는 정적 변수
- Stack : 함수의 호출, 반환에 따라 쌓이고 줄어드는 메모리 영역
- Heap : 프로그램이 사용하는 동적 변수( 프로그램이 변수 공간을 할당하면 이곳에 할당 )

각 프로세스는 서로다른 PID가 있고, OS가 안정성을위해 다른 프로세스가 침범하지 못하도록 제약을 두고있다. 다른 프로세스의 변수나 자료 구조에 접근하려면 프로세스 간의 통신을 사용해야 한다.

### 프로세스 context

운영체제는 하나의 CPU로 여러 개의 프로세스를 구동하기 위해 시분할 방식을 사용한다
CPU는 매우 빠른 속도로 연산할 수 있으므로 아주 잠깐씩 여러개를 실행하면, 사용자는 동시에 여러개가 실행되는 것으로 느낀다.

이때 프로세스 문맥(context) 라는것이 사용되는데 이전의 CPU보유 시점에서 프로세스의 상태를 재현하기 위해 사용된다

현재까지 무엇을 어떻게 실행했는지 정확하게 나타내기 위해 사용된다

- ex) Code에서 함수호출시 stack에 쌓인내용, data영역에서 변경된 변수의 값, 레지스터에 어떤 값을넣고 어떤 명령까지 실행했는가 등에 대한 정보

### 스케줄링

어떤 프로세스를 더 먼저 or 더 길게 실행할 것인지 스스로 판단하며 이에 관련된 다양한 CPU 스케줄링 기법이 존재한다.

장기,중기,단기 3가지 단계의 각각 알고리즘이 많이있다 선입선출, 라운드로빈 등

## 쓰레드

프로세스가 할당받은 자원을 이용하는 흐름 단위이다. 

여러개의 쓰레드가 있을수 있으며 같은 프로세스 안에서 쓰레드끼리는 Code, Data, Heap 자원을 공유하면서 Stack만 따로 할당받기때문에 한 쓰레드가 자원을 변경할시에 다른 쓰레드에서 즉각 그 결과를 알수 있다.
단, 자원을 공유하므로 동기화 문제가 발생할 수 있다.
즉 어플리케이션 하나가 프로세스이고, 그 안에서의 분기 처리가 쓰레드가 된다.

또한 프로세스 간의 전환 속도보다 쓰레드 간의 전환 속도가 빠르다.

## 멀티프로세스 vs 멀티쓰레드

- 메모리 효율 → 멀티쓰레드 Win!
- 상호간 통신 → 멀티쓰레드 Win!
- 안정성 → 멀티프로세스 Win!

스레드 사이의 통신은 공유 메모리를 통해 쉽게 통신할 수 있지만, 프로세스 사이의 통신은 IPC등의 기술을 사용해야 한다.

결합도의 관점에서, 멀티스레드 사용시 동기화 이슈가 발생할 수 있다.

또한 멀티쓰레드는 스케줄링을 운영체제가 자동으로 해주지 않고, 프로그래머가 적절한 기법을 선택하여 직접 구현해야하므로 프로그램을 멀티 스레드로 개발할 때는 신중해야한다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)
<br><br>

## 스케줄러

### 스케줄러(Scheduler)란?

- **한정적인 메모리를 여러 프로세스가 효율적으로 사용할 수 있도록** 다음 실행 시간에 실행할 수 있는 프로세스 중에 **하나를 결정**하는 운영체제 커널의 코드이다.
- **프로세스를 관리하는 Queue**에는 다음 세 가지 종류가 있다.

> `Job Queue` :  현재 시스템 내에 있는 **모든 프로세스의 집합**
>
> `Ready Queue` : 현재 메모리 내에서 **CPU를 할당받기를 기다리는 프로세스의 집합**.  앞에 있는 순서대로 OS로부터 CPU를 할당받아 실행된다.
>
> `Device Queue` : **Device I/O 작업을 대기하고 있는 프로세스의 집합**

![image](https://user-images.githubusercontent.com/76988389/134460682-49f86127-ea73-4373-88c4-cf7ad340c0a2.png)

### 장기 스케줄러(Long-Term Scheduler)

- 지금 수행해야 할 job Pool 중에 어떤 것을 메모리에 올릴지 결정한다.**(디스크 - 메모리 스케줄링)**
- 프로세스에 메모리 및 각종 리소스를 할당한다.**(admit)**
- job을 골라서 ready queue에 보내기 때문에 **job scheduler**라고도 부른다.
- CPU의 계산 작업이 필요한 **CPU bound process**만 메모리에 올라가면 사용자와 실시간 상호작용이 어렵다.
- **I/O bound process**만 메모리에 올라가면 입출력을 기다리느라 CPU가 놀게 된다.
- 이를 적절한 비율로 메모리에 적재하는 것이 장기 스케줄러의 역할이다.
- 이 때, 프로세스의 상태는 **new -> ready(in memory) 상태**가 된다.

- **호출빈도 매우 적다.** 또한, **오늘날**에는 가상 메모리 관리(virtual memory management)가 발달해서 모든 job이 메모리에 올라가므로, **장기 스케줄러는 거의 사용되지 않는다.**



### 단기 스케줄러(Short-Term Scheduler)

- CPU는 한 번에 하나의 process만 수행한다.
- 장기 스케줄러에 의해 메모리에 올라간 process들 중, 어떤 걸 먼저 처리할지 결정하는 것이 단기 스케줄러의 역할이다.**(메모리 - CPU 스케줄링)**
- Ready Queue의 프로세스 중 어떤 프로세스에 CPU를 할당해서 running할 지 결정한다.**(scheduler dispatch)**
- 가상 메모리 관리의 발달로 Short-Term/Long-Term 구분이 없는 요즘은, **CPU scheduler**라고도 부른다.
- 이 때, 프로세스의 상태는 **ready -> running -> waiting -> ready**가 된다.
- **호출빈도가 높다.** 특히 **오늘날** 단기 스케줄러로 거의 모든 프로세스가 제어된다.



### 중기 스케줄러(Mid-Term Scheduler)

- CPU는 한 번의 하나의 process만 수행하는데, 사용자의 다중 작업을 지원하기 위해 메모리에 올라간 작업들 중 단기 스케줄러에 의해 먼저 처리할 작업을 선정하고, 아주 짧은 시간동안 수행하기를 반복한다.
- 즉, 단기 스케줄러가 수행할 process를 정해준다고 해도, 그 process가 끝날때까지 계속 수행하는게 아니다.
- 따라서 **CPU가 감당하기 어려울 만큼의 job이 메모리에 올라오면, 우선순위가 낮은 job은 잠시 내려뒀다가 나중에 다시 올리는 (swap) 작업을 통해 작업 효율을 높일 수 있다.**
- 이 일을 해주는 것이 중기 스케줄러이다. 역할 때문에 **swapper**라고도 부른다.
- 프로세스의 상태는 **ready -> suspended**가 된다.
- **장기 스케줄러와 마찬가지로 가상 메모리 관리가 발달하면서 사용되지 않는다.**

> `Process state - suspended`
>
> Suspended (stopped) : 외부적인 이유로 프로세스의 수행이 정지된 상태로 메모리에서 내려간 상태 의미한다. 프로세스 전부 디스크로 swap out 된다. blocked 상태는 다른 I/O 작업을 기다리는 상태이기 때문에 스스로 ready state로 돌아갈 수 있지만 이 상태는 외부적인 이유로 suspending 되었기 때문에 스스로 돌아갈 수 없다.


[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)
<br><br>

## 멀티쓰레드(장점, 단점)

### 멀티쓰레드의 의미
하나의 응용 프로그램에서 여러 쓰레드를 구성해 각 스레드가 하나의 작업을 처리하는 것.
쓰레드 간 자원을 공유하고 자원의 생성과 관리의 중복성을 최소화하여 수행 능력을 향상 시킨다.
> 멀티 프로세스 : 여러 개의 CPU를 사용하여 어러 프로세스를 동시에 수행하는 것.

### 멀티쓰레드 장점
- 응답성 : 프로그램의 일부 작업이 중단되거나 처리 과정이 오래 걸려도 프로그램의 수행이 계속되어 사용자에 대한 응답성이 증가한다.
- 멀티프로세서 활용 : 다중 CPU 구조에서는 각각의 쓰레드가 다른 프로세서에서 병렬로 수행될 수 있다.
- 쓰레드는 프로세스 내의 Stack 영역을 제외한 모든 메모리를 공유한다.
	- 메모리나 시스템 자원의 낭비가 적다. (중복되는 자원의 생성과 관리 최소화)
	- 통신이 필요한 경우에 전역 변수 또는 힙 영역을 이용하여 쉽게 데이터를 주고받을수 있다.
- 쓰레드의 context switch는 캐시 메모리를 비울 필요가 없다.
	- 시스템 처리량이 향상되고 자원 소모가 줄어들어 프로그램의 응답 시간이 단축된다.
- 코드 공유를 통해 한 응용프로그램 같은 주소공간내에서 여러개의 다른 활동성 쓰레드를 가질 수 있다.


### 멀티쓰레드 단점
- 동시접근 문제 : 서로 다른 쓰레드가 데이터와 힙 영역을 공유하므로 한 쓰레드가 다른 쓰레드에서 사용중인 변수나 자료구조에 접근하여 엉뚱한 값을 읽어오거나 수정할 수 있다.
- 멀티 쓰레드 프로그램은 실행 순서가 보장되지 않는다.
- 동기화(Mutex, Semaphore)를 통해 작업처리나 메모리 접근을 컨트롤 할 수 있으나 설계가 어려우며, 불필요한 부분까지 동기화를 할 경우 과도한 LOCK으로 인해 병목현상을 일으켜 성능이 저하될 수 있다
- 싱글코어에서의 멀티쓰레드의 경우 오히려 쓰레드 생성시간이 오버헤드로 작용하여 단일쓰레드보다 느리다.

> - Mutex
>   - 쉽게 말해 Key가 하나뿐인 메커니즘
>   - Key를 이용하여 접근컨트롤 Key가 있는 쓰레드만이 자원에 접근가능
>   -접근이 끝났을때 wait를 호출한 쓰레드만 해제(Key넘기기) 가능
>
> - Semaphore
>   - 쉽게 말해 Key가 여러개인 메커니즘
>   - Lock을 걸지 않은 쓰레드(wait호출x)도 Key넘기기 가능

### 멀티 프로세스 vs 멀티 쓰레드

|      |                멀티 프로세스 (Multi Process)                 |                  멀티 쓰레드 (Multi Thread)                  |
| :--: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 장점 | 하나의 프로세스가 죽더라도 다른 프로세스에는 영향을 끼치지 않고 정상적으로 수행된다. | 멀티 프로세스보다 적은 메모리 공간을 차지하고 문맥 전환(context switching)이 빠르다. |
| 단점 |   멀티 쓰레드보다 많은 메모리 공간과 CPU 시간을 차지한다.    | 오류로 인해 하나의 쓰레드가 종료되면 전체 스레드가 종료될 수 있다는 점과 동기화 문제가 있다. |

> 💡 Context Switching이란?
  >
  > 프로세스의 상태 정보를 저장하고 복원하는 일련의 과정
  >
  > 즉, 동작 중인 프로세스가 대기하면서 해당 프로세스의 상태를 보관하고, 대기하고 있던 다음 순번의 프로세스가 동작하면서 이전에 보관했던 프로세스 상태를 복구하는 과정을 말한다.
  >
  > 프로세스는 각 독립된 메모리 영역을 할당받아 사용되므로, 캐시 메모리 초기화와 같은 무거운 작업이 진행되었을 때 오버헤드가 발생하는 문제가 존재한다.

멀티 쓰레드와 멀티 프로세스는 동시에 여러 작업을 수행한다는 점에서 같지만 적용해야 하는 시스템에 따라 적합/부적합이 구분된다. 따라서 대상 시스템의 특징에 따라 적합한 동작 방식을 선택하고 적용해야 한다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)
<br><br>

## 프로세스 동기화

### 프로세스 동기화란?

- 하나의 자원을 한 순간에 하나의 프로세스만이 이용하도록 제어하는 것을 말한다.
- 공유 자원에 대해 여러 프로세스가 동시에 접근할 때, 결과값에 영향을 줄 수 있는 상황을 `Race Condition(경쟁 조건)`이라고 한다.
- 프로세스/스레드 동기화를 통해 `데이터의 일관성을 유지`할 수 있다. (결합도↓)



[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)



### 경쟁 조건

다음 상황들에 있어서 경쟁 조건이 발생할 수 있다.

**커널 수행중 인터럽트 발생**

`Solution` 커널 수행중에 인터럽트가 걸려도 무시하고 작업이 끝난 후 처리

**프로세스가 System Call 하여 커널 모드로 수행 중 context switch가 일어나는 경우**

`Solution` 커널 모드에 있는 경우 CPU 시간이 끝나도 할당된 CPU를 작업 종료시까지 유지

**멀티 프로세서에서 공유 메모리 내의 커널 데이터에 접근**

`Solution` 커널 내부의 각 공유 데이터에 접근할 때마다 해당 데이터를 lock/unlock (접근 차단/해제)



[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)



### 스레드 동기화란?

- 하나의 코드블록 또는 메소드를 한 순간에 하나의 스레드만이 이용하도록 제어하는 것을 말한다.



[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)



### 구역(Section)

- `임계구역(critical section)` : 공유 자원에 접근하는 코드의 일부를 말한다.
- `입장구역(entry section)` : 각 프로세스가 자신의 임계구역으로 진입할 때 진입 허가를 요청하는 부분을 말한다.
- `퇴장구역(exit section)` : 코드 실행을 마친 후 임계구역을 나오기 전 실행해야 하는 코드를 말한다.
- `나머지구역(remainder section)` : 다른 프로세스 및 공유데이터와 상관이 없는 코드를 말한다.



[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)



### 임계구역 문제

**임계구역으로 지정되어야 할 코드 영역이 임계구역으로 지정되지 않았을 때 발생하는 문제**를 말한다.

**임계구역 문제를 해결하기 위한 필요조건**은 다음과 같다.

- **상호 배제(Mutual exclusion)** : 하나의 프로세스가 임계구역에 들어가 있다면 다른 프로세스는 들어갈 수 없다.
- **진행(Progress)** : 임계구역에 들어간 프로세스가 없을 때, 들어가려는 프로세스가 여러 개 있다면 어느 것이 들어갈지를 적절히 결정해줘야 한다.
- **한정 대기(Bounded waiting)** : 모든 프로세스가 평등하게 공유 데이터에 접근하는 것을 보장하기 위해, 한 번 임계 구역에 들어간 프로세스는 다음 번 입장 시 제한을 두어야 한다.



[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)



### 임계구역 문제 해결책

**소프트웨어적인 방법** 

- 프로세스가 2개일 때
  - `피터슨의 알고리즘` : flag값을 이용한 상호 배제를 만족하는 병렬 프로그래밍 기법이다.
```java
do{
flag[i]=true;// i 가 사용하고싶어함
trun =j;// i가 쓰고싶다고 설언해주고 j차례로 돌려서 혹시 다른프로세스가 쓰고싶나 확인
while(flag[j]&&trun==j);//자기 턴일때까지 spinlock에 머물러야함 여기서 cpu낭비를함
//임계영역 사용
flag[i]=false;//사용이 끝나면 해제
//나옴
}while(true)
```
  - `데커의 알고리즘` : 피터슨의 알고리즘과 비슷하다.
- 프로세스가 2개 이상일 때
  - `램포트의 빵집 알고리즘`

**하드웨어적인 방법**

- `동기화 명령어` 멀티 프로세서 환경에서는 시스템 효율성 때문에 인터럽트를 막을 수 없으므로, CPU 차원에서 지원하는 동기화 명령어를 활용하는 방법이다. 주로 `TestAndSet()`과 `Swap()` 명령어로 이를 구현한다.
- `Semaphores(세마포어)` 원자적 함수를 통해 제어되는 변수값을 말한다.
  - 이진 세마포어 : Mutex LOCK처럼 0이면 락 1이면 unlock
  - 카운팅 세마포어 : n개의 자원을 locking 하나의 자원할당시 n-1, 두개는 n-2 이런식으로 0까지 할당가능
  - 둘이상의 프로세스가 무한정 대기하는 Dead Lock에 걸릴 수 있음 이때는 시간을 우선순위로 두어 해결
- `모니터` 고급 언어에서 지원하는 기능 또는 모듈로써, 한 번에 하나의 프로세스만 활동하도록 보장해준다. (개발자의 코드를 상호 배제하도록 추상화된 데이터 형태)
- `LOCK/UNLOCK`



[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)


<br>
<br>


## 동기와 비동기의 차이

동기와 비동기는 어떤 작업 혹은 그와 연관된 작업을 처리하고자 하는 시각의 차이이다.

### 동기(Sync, synchronous)
직렬적으로 태스크(task)를 수행한다. 즉, 태스크는 순차적으로 실행되며 어떤 작업이 수행 중이면 다음 작업은 대기하게 된다.  
설계가 간단하고 직관적인 대신, 결과가 주어질 때까지 대기해야 한다는 단점이 있다.

![동기](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcs2zHt%2FbtqF1aJFTdB%2FsP2Pzm6ZAiNnpUKzJvMJLK%2Fimg.png)

- 메소드를 실행시킴과 동시에 반환 값이 기대되는 경우
    - 요청과 결과가 한 자리에서 **동시에 일어난다**.
    - 요청을 보낸 후 결과를 받아야지만 다음동작이 이루어지는 방식이다.  
    ⇒ 요청 - 응답 과정이 연속적으로 일어남 (요청 - 응답 - 요청 - 응답 - ...)
- 요청과 그 결과가 동시에 일어나기 때문에 **결과가 주어질 때까지 대기**해야 한다.
    - 값이 반환되기 전까지 다른 프로그램은 bloking(정지)되어 있다. (트랜젝션을 맞추기위함)
    - 예) 클라이언트가 서버에 어떤 걸 요청하면, 서버에서 처리해서 응답을 반환해줄 때까지 클라이언트는 아무것도 못하고 대기함
- A노드와 B노드 사이의 작업 처리 단위(transaction)를 동시에 맞춘다.
- 설계가 매우 간단하고 직관적이다.
- CPU가 느려지는것은 아니지만 효율이 저하된다.

<br>

### 비동기(Async, asynchronous)
비동기식 처리는 병렬적으로 태스크를 수행한다. 즉, 태스크가 종료되지 않은 상태라 하더라도 대기하지 않고 다음 태스크를 실행한다.  
설계가 동기보다 복잡하지만, 결과가 주어지는데 시간이 걸리더라도 그 시간 동안 다른 작업이 가능해 자원을 효율적으로 사용할 수 있다. 

![비동기](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdkpuUa%2FbtqF2RhRoUu%2FUaSnCg4Rk0EaLj6fAjQ6H0%2Fimg.png)

- 요청 - 응답 과정이 **동시에 일어나지 않는다**. (요청 - 요청 - 응답 - 요청 - 응답 - 응답)
    - 즉, 요청한 그 자리에서 결과가 주어지지 않음
    - 요청과 응답이 목적이 다를때 사용 가능하다. ( 꼭 결과를 바로 알아야하는 경우가 아닐 때)
    - 비동기는 요청 후 값이 반환되기전까지 blocking되지 않고 이벤트 큐에 넣거나 백그라운드 스레드에게 해당 task를 위임한다. 그리고 바로 다음 코드를 실행하기 때문에 **기대되는 값이 바로 반환되지 않는다.**
    - 예) 클라이언트가 서버에 어떤 걸 요청하면, 서버는 응답을 처리하는데 결과가 주어지기 전까지 다른 요청을 받을 수 있음. 즉, **비동기에서 클라이언트는 앞 요청의 응답에 상관 없이 또다른 요청을 할 수 있다.**
- 노드 사이의 작업 처리 단위를 동시에 맞추지 않아도 된다.
- 동기보다 설계가 복잡하다.
- DOS같은 단일 OS에서는 불가능하고 windows같은 멀티태스크 환경에서만 가능하다.
- 결과가 주어질 때까지 다른 작업을 할 수 있다.

> BlockingI/O 작업을 진행하는 동안 유저 프로세스의 작업을 중단시킨다.  
> Non-BlockingI/O 작업을 진행하는 동안 유저 프로세스의 작업을 중단시키지 않는다.

<br>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)

<br>

### 동기와 비동기 예시

![image](https://user-images.githubusercontent.com/76988389/134413249-841d78d3-c9a7-4a72-a9c8-48697d16dad7.png)

- 그림에서 아래의 예는 직원이 두 명이기 때문에 정확히는 비동기가 아니라 멀티프로세싱이라고 보는게 맞아요
- 동기적 관점 : 손님이 커피를 주문 - 직원이 커피를 내림 - 커피가 만들어지면 손님에게 반환 - 다음 손님이 주스를 주문 - 직원이 주스를 만듦 - 주스가 만들어지면 손님에게 반환 - ...
- 비동기적 관점 : 손님이 커피를 주문 - 직원이 커피액상 추출기에 원두와 물을 넣어놓음 - 다음 손님이 주스를 주문 - 직원이 과일원액 추출기에 과일을 넣어놓음 - 다음 손님(요청)이 없음 - 커피를 반환 - 주스를 반환
- 멀티프로세싱 관점 : 주문받는 직원과 음료를 만드는 직원이 따로 있음
- 멀티스레딩 관점 : 한 명의 직원이 그림자분신마냥 두 명의 직원으로 보일 만큼 빠른 속도로 주문과 음료제작을 모두 혼자 처리하는 것


<br>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)

<br>

### 동기식 입출력 vs 비동기식 입출력
#### 동기식 입출력
입출력이 진행되는 동안 해당 프로그램이 다음 명령을 수행하지 않고 기다린다.
인터럽트를 통해 입출력이 완료되었음을 알리면 CPU의 제어권이 프로그램으로 넘어가서 다음 명령을 수행할 수 있다.
⇒ CPU는 입출력이 끝날 때까지 인터럽트를 기다리며 자원 낭비
⇒ 일반적으로 입출력을 수행할 때 CPU를 다른 프로그램에게 이양해 CPU가 계속 일할 수 있도록 관리함

1. 입출력이 완료될 때까지 해당 프로그램에 CPU를 재할당하지 않는다.
2. 이를 관리하기 위해 입출력 중인 프로그램을 `봉쇄 상태(blocked state)`로 전환시킨다.
3. 봉쇄 상태가 아닌 프로그램에만 CPU를 할당한다.

#### 비동기식 입출력
입출력 연산을 요청한 뒤 CPU의 제어권을 입출력 연산을 호출한 프로그램에 바로 다시 부여하는 방식
- 입출력 요청과 상관 없이 수행할 수 있는 작업을 먼저 수행
- 자원을 효율적으로 사용

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)
<br><br>

## CPU 스케줄러

스케줄러는 Ready Queue에 있는 프로세스들을 특정한 우선순위를 기반으로 CPU를 할당받게 해주는 역할을 한다.

- 공평성 : 모든 프로세스가 자원을 공평하게 배정받아야 하며, 특정 프로세스가 배제되어서는 안 된다.
- 효율성 : 시스템 자원을 놀리는 시간 없이 스케줄링해야 한다.
- 안정성 : 우선순위를 사용하여 중요한 프로세스가 먼저 처리되도록 해야 한다.
- 반응 시간 보장 : 응답이 없는 경우 사용자는 시스템이 멈춘 것으로 가정하기 때문에 시스템은 적절한 시간 안에 프로세스의 요구에 반응해야 한다.
- 무한 연기 방지 : 특정 프로세스의 작업이 무한히 연기되어서는 안 된다.

### 비선점 스케줄링 (Non-preemptive scheduling)

Time-slice 가 없는 스케줄링
CPU를 사용중인 프로세스가 자율적으로 반납하도록 하는 방식
프로세스 종료 or I/O 등의 이벤트가 있을 때까지 실행을 보장하지만 처리시간 예측이 어렵다.
일반적으로 Interrupt, I/O or Even Completion 이 발생한 시점에서 사용하지만 상황에 따라 I/O or Event Wait, Exit 상황에서도 스케줄링이 가능하다.

### 선점 스케줄링 (Preemptive scheduling)

낮은 우선순위를 가진 프로세스보다 높은 우선순위를 가진 프로세스가 CPU를 선점하는 방식
OS가 스케줄링의 알고리즘에 따라 적당한 프로세스에게 CPU를 할당하고, 필요시에는 회수하는 방식
프로세스가 자율적으로 CPU를 반납하는 시점(I/O or Event Wait, Exit)에서 사용한다.


> 프로세스의 상태 전이
>
> ![프로세스 상태](https://t1.daumcdn.net/cfile/tistory/990DB03F5C7AC10303)
>
> - 승인 (Admitted) : 프로세스 생성이 가능하여 승인됨
> - 스케줄러 디스패치 (Scheduler Dispatch/Scheduled) : 준비 상태에 있는 프로세스 중 하나를 선택하여 실행시키는 것
> - 인터럽트 (Interrupt/Time-slice burst) : 예외, 입출력, 이벤트 등이 발생하여 현재 실행 중인 프로세스를 준비 상태로 바꾸고 해당 작업을 먼저 처리하는 것
> - 입출력 또는 이벤트 대기 (I/O or Event wait) : 실행 중인 프로세스가 입출력이나 이벤트를 처리해야 하는 경우, 입출력/이벤트가 모두 끝날 때까지 대기 상태로 만드는 것
> - 입출력 또는 이벤트 완료 (I/O or Event Completion) : 입출력/이벤트가 끝난 프로세스를 준비 상태로 전환하여 스케줄러에 의해 선택될 수 있도록 만드는 것


### CPU 스케줄링 종류

- ### 비선점 스케줄링 (Non-preemptive Scheduling)

1. FCFS (First Come First Served)

   - 먼저 온 고객을 먼저 서비스해주는 방식. 즉, 먼저 온 순서대로 처리.

   - CPU를 사용하고 있는 프로세스 자기 자신이 CPU를 놓아줄 때까지 CPU를 다른 프로세스에게 양보하지 않는다.

   - 소요시간이 긴 프로세스가 먼저 도달하여 효율성을 낮추는 현상이 발생한다.(convoy effect)



2. SJF (Shortest Job First)

   - 수행시간이 짧은 프로세스부터 CPU할당

   - 늦게 도착하더라도 CPU 처리 시간이 앞에 대기중인 프로세스보다 짧으면 먼저 CPU를 할당받을 수 있기 때문에 콘보이 효과를 완화할 수 있다.

   - CPU 점유 시간 자체를 예측해야만 구현할 수 있다.

   - CPU버스트가 긴 프로세스는 준비 큐에서 무한정 대기해야 할 수 있다. => 기아현상



3. HRN (Highest Response-ratio Next)

   - 대기 중인 프로세스 중 현재 응답률(Response Ratio)이 가장 높은 것을 선택

   - SJF의 약점인 starvation을 보완한 기법으로 긴 작업과 짧은 작업 간의 불평등 완화

   - HRN의 우선순위 = (대기시간 + 서비스 시간) / 서비스 시간


4. Priority Scheduling

   - 우선순위가 가장 높은 프로세스에게 CPU를 할당하는 스케줄링

   - 우선순위란 정수로 표현하게 되고 작은 숫자가 우선순위가 높다.

   - 우선순위가 같은 경우에는 FCFS 방식을 적용한다.

   - 선점, 비선점 스케줄링 모두에 사용된다.

     - 선점형 스케줄링 방식 : 더 높은 우선순위의 프로세스가 도착하면 실행 중인 프로세스를 멈추고 CPU를 선점한다.
     - 비선점형 스케줄링 방식 : 더 높은 우선순위의 프로세스가 도착하면 ready queue의 head에 넣는다.

   - 문제점

     - starvation

     - 무기한 봉쇄 (Indefinite blocking)

      실행 준비는 되어있으나 CPU를 사용하지 못하는 프로세스가 CPU를 무기한 대기하는 상태

   - 해결책

     - Aging 기법

       레디 큐에서 오랜 시간을 기다린 프로세스는 계속해서 우선순위를 높히는 기법

- ### 선점 스케줄링 (Preemptive Scheduling)

1. SRT (Shortest Remaining Time First)
   - 새로운 프로세스가 도착할 때마다 새로운 스케줄링이 이루어진다.

   - SJF의 선점형 방식이다.

   - 먼저 온 프로세스가 CPU를 할당받고 있더라도 남은 처리 시간이 뒤에 온 프로세스의 처리 시간보다 길면 CPU를 빼앗긴다.

   - 어떤 알고리즘보다 평균 대기 시간이 가장 짧다.

   - 선점형 방식이기 때문에 잦은 문맥교환이 일어나고 그에 따른 오버헤드가 커진다.
   - starvation 현상이 더 심각하게 발생할 수 있다.
   - CPU의 예상 시간을 예측하기 힘들기 때문에 실제로 사용되기는 어렵다.

2. RR (Round Robin)

   - 현대적인 CPU 스케줄링
   - 각 프로세스는 동일한 크기의 할당 시간(time quantum)을 갖게 된다.
     - 할당 시간이 지나면 프로세스는 선점당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다.
     - 프로세스가 기다리는 시간이 CPU를 사용할 만큼 증가한다. 공정한 스케줄링이라고 할 수 있다.
   - CPU 처리 시간을 계산하지 않아도 돼서 선점형 방식의 가장 단순하고 대표적인 방법이다.

   - RR이 가능한 이유는 프로세스의 context를 save할 수 있기 때문이다.

   - Response time이 빨라진다.
      - n개의 프로세스가 ready queue에 있고 할당시간이 q(time quantum)인 경우 각 프로세스는 q 단위로 CPU 시간의 1/n을 얻는다. 즉, 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.

     - 할당시간 : CPU를 연속적으로 사용할 수 있는 최대 시간
     - 할당시간이 너무 길면 FCFS와 같은 결과가 발생한다.
     - 할당시간이 너무 짧으면 CPU를 사용하는 프로세스가 빈번하게 교체되어 문맥교환의 오버헤드가 커진다.
     - 따라서 일반적으로 할당시간은 수십ms 정도의 규모로 설정한다.

   - 일반적으로 SJF보다 평균 대기시간은 길지만 응답시간은 더 짧다. SJF보다 공정하다.
<br>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)

<br />

## 메모리 관리 전략1 (메모리 관리 배경)

### 메모리 관리 배경
운영체제와 시스템을 보호하기위해 독립된 메모리 공간을 보장해야한다
운영체제는 각 프로세스들을 메모리 각각의 공간에 할당하고, 프로세스가 운영체제 혹은 다른 프로세스의 메모리 공간에 접근하는 것을 막는다.
운영체제만이 운영체제 메모리 영역과 사용자 메모리 영역의 접근에 자유롭다.
다수의 프로세스를 사용하면서 효율적인 메모리관리의 중요성이 대두되었다.

### 메모리 관리 전략
메모리는 CPU가 직접 접근하는 유일한 저장장치로 메모리 시스템(하드웨어)은 주소(메모리 위치)를 관리하며 할당과 접근을 제어한다.

#### 메모리 관리 전략의 목적
- 제한된 물리 메모리의 효율적인 사용 (할당)
- 효율적인 메모리 참조 (논리-물리 주소 할당)

### 논리 <-> 물리주소 공간

- Cpu 가 생성하는 주소를 논리주소, 메모리가 취급하는 주소를 물리주소라 한다.
	- 논리적 주소 : 각 프로세스마다 0번지부터 시작, CPU가 명령문을 실행함으로써 발생하는 주소
	- 물리적 주소 : 메모리에 실제 올라가는 위치
- 프로그램 실행 중에는 이와같은 가상주소를 물리주소로 바꿔줘야 하는데, 이 매핑작업은 메모리 관리기(Memory Management Unit)에서 실행된다.

### 주소 바인딩 - 프로그램 순서에 따른 바인딩

- 주소 바인딩이란 프로세스가 접근해야 하는 값과 함수에 대한 주소가 정해지는 것을 말한다.
- 주소변환은 순전히 하드웨어의 임무, 운영체제는 주소변환에 관여하지 않는다.
- 논리 주소와 물리 주소는 프로그램이 실행되는 과정 중 어느 시점에 바인딩이 되는지에 따라 다르다.

1. **소스코드**
    - 컴파일 시점 주소 바인딩 <br>
       컴파일 될 때 컴파일러에서 명령어들과 변수의 메모리 주소가 정해지는 것. 현재 os는 사용 안함. <br>

 	   `논리 주소 == 물리 주소`

2. **실행파일**
   - 로드 시점 주소 바인딩 <br>
     프로그램 실행 전에 물리 주소 할당, 컴파일러가 재배치 가능한 코드를 생성한 경우 가능 <br>
	   `논리 주소 == 물리 주소` 

3. **실행시작**
   - 런타임 시점 주소 바인딩 <br>
     매번 명령어가 실행될때마다 주소가 계산되는 것, 실행 중에도 메모리 위치 이동 가능 <br>
     cpu가 주소를 참조할 때 마다 바인딩 점검, 하드웨어 지원 필요(base & limit register, **MMU**) <br>
	   `논리 주소 != 물리 주소`

### Swapping
프로세스가 실행되기 위해서는 메모리에 있어야 하지만, 스케줄링의 다중 프로그래밍 환경에서 CPU 할당 시간이 끝난 프로세스는 실행 중에 임시로 예비저장장치(backup store)에 내보내졌다가 실행을 계속하기 위해 다시 메모리로 돌아올 수 있다.

![swapping 과정](https://mblogthumb-phinf.pstatic.net/MjAxOTA4MTdfMTQ5/MDAxNTY2MDA5ODc2OTEw.YPE34xyH7dWIO3zZ7RPZ069uGU44qV66o1MVaGZOOmYg.yt4Yrna18d_whYtB16j-5ThGV9fOZmNmOMhNzLhqHfgg.PNG.bisu1532/gfdsfdasda.PNG?type=w800)

- 스와핑
  - 프로세스를 일시적으로 메모리에서 백킹 스토어로 내보내는 것
- 백킹 스토어(swap area)
  - 디스크: 많은 사용자의 프로세스 이미지를 담을 만큼 충분히 빠르고 큰 저장 공간
- 프로세스를 주기억장치(RAM)으로 불러오는 과정을 `swap-in`, 보조 기억장치로 내보내는 과정을 `swap-out` 이라고 한다.
	- 주로 중기 스케줄러에 의해 아웃시킬 프로세스 선정
	- 우선순위에 따른 관리
	- 효율적인 관리를 위해 런타임 바인딩이 지원돼야함
- swap에는 큰 디스크 전송 시간이 요구되기 때문에 현재엔 메모리 공간이 부족할 때 swapping이 시작된다.
- swap-out된 프로세스가 다시 메모리에 올라갈 때는 원래 있던 자리로 돌아가지 않을 수 있다.
- 상당한 Context-switching time이 발생한다.
- **<u>프로세스가 종료되어 그 주소 공간을 디스크로 보내는 것이 아닌, 메모리에 존재하는 프로세스의 수를 조절하기 위해 수행 중인 프로세스의 주소 공간을 일시적으로 메모리에서 디스크로 내려놓는 것!</u>**

### 물리적 메모리 할당 방식
#### 연속 할당 방식
프로세스를 메모리에 올릴 때, 물리적 메모리의 한 곳에 연속적으로 적재하는 방식
- 고정분할 방식
	- 물리적 메모리를 주어진 개수만큼 영구적인 분할로 미리 나누어 두고, 각 분할에 하나의 프로세스를 적재하는 것
	- 분할의 크기는 다를 수 있다.
	- 동시에 메모리에 올릴 수 있는 프로그램 수가 고정되어 있고, 수행 가능한 프로그램의 최대 크기가 제한된다.
	- 외부조각, 내부조각 발생 가능
- 가변분할 방식
	- 메모리에 적재되는 프로그램의 크기에 따라 분할의 크기와 개수가 동적으로 변하는 방식
	- 내부조각은 발생하지 않지만, 외부조각이 발생 가능
	- 프로세스를 메모리에 올릴 때 어디에 올릴 지 결정하는 '동적 메모리 할당 문제'와 외부조각을 해결해야 되는 문제가 있다.

#### 불연속 할당 방식
- 페이징
- 세그멘테이션

### 단편화 (Fragmentation)

프로세스들이 차지하는 메모리 사이사이에 사용하지 못할 크기의 작은 빈 공간들이 생기는 것.

![Fragmentation](https://i.stack.imgur.com/dSWgj.gif)

- 외부 단편화
	- 물리 메모리 사이에서 남는 작은 공간들을 모두 합치면 의미 있는 공간이 되지만, 현재는 작은 부분들로 분산되어 있을 때 발생.
- 내부 단편화
	- 프로세스가 사용하는 메모리 공간에 포함된 남은 부분. 요구된 메모리 보다 약간 커서 빈 곳이 쓸모는 없지만 비어있는 상태.
- 압축
  - 외부 단편화를 해소하기 위해 프로세스가 사용하는 공간들을 한 쪽으로 몰아 자유공간을 확보하는 방법. 
  - 프로세스들의 재배치가 실행 시간에 동적으로 이루어지는 경우에만 가능하고 작업 효율이 좋지 않다.
  
### (참고) 관련된 용어
#### 동적로딩
- 다중 프로그래밍 환경에서 메모리 사용의 효율성을 노핑기 위해 사용하는 기법 중 하나
- 프로세스가 시잘될 때 프로세스의 주소 공간 전체를 메모리에 다 올리지 않고, 실행에 필요한 부분이 불릴 때 메모리에 적재하는 방식
- 사용되지 않는 많은 양의 코드가 메모리에 올라가는 것을 막아 메모리 효율을 높임
- 프로그램 자체에서 구현하거나, 운영체제의 라이브러리를 통해 지원

#### 동적연결
> **연결(linking)**  
> 목적파일(object)과 라이브러리파일(library)를 묶어 하나의 실행파일을 생성하는 과정

> **cf) 정적연결**  
> 프로그래머가 작성한 코드와 라이브러리가 모두 합쳐져서 실행파일이 생성됨. 실행파일의 크기가 상대적으로 크며, 물리적 메모리가 낭비됨

- 연결(linking)과정을 프로그램의 실행 시점까지 지연시키는 기법
- 즉, 실행 파일에 라이브러리 코드가 포함되지 않으며, 실행 중 라이브러리 함수를 호출할 때 라이브러리에 대한 연결이 이루어짐
- 라이브러리 위치를 찾기 위해, 실행파일의 라이브러리 호출 부분에 스텁(stub)이라는 작은 코드를 둔다.
- 다수의 프로그램이 공통으로 사용하는 라이브러리를 한 번만 적재하므로 메모리의 효율
- 운영체제의 지원이 필요함

#### 중첩
- 프로세스의 주소 공간을 분할해 실제 필요한 부분만 메모리에 적재하는 기법
- 초창기 물리적 메모리의 크기가 작을 때 어쩔 수 없이 필요한 부분만 메모리에 올려 실행하고, 나머지 부분을 올려 실행하던 기법 

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)

<br />

## 메모리 관리 전략2 (Paging)

### 배경

`외부단편화(external fragmentation)`는 프로그램에 할당되지는 않았지만, 크기가 작아서 프로그램을 올리지 못하고 낭비되는 영역이다. 이를 해결하기 위해 `압축(Compaction)`을 실행하지만, 수행중인 프로세스들의 메모리 위치를 많이 이동시키므로 비용이 크다. 

외부단편화는 process가 조각난(남은) 메모리보다 커서 발생한다. 따라서 페이징은 '조각난 메모리와 프로세스의 크기를 동일하게 맞춰줘서 하나씩 매칭한다면 공간이 들어맞지 않을까?' 라는 아이디어에서 파생되었다. 프로세스의 수행은 연속적으로 되어야 하므로 이전까지는 프로세스가 연속적으로 할당되어야한다는 관념이 있었고, 외부단편화가 생겼다. 하지만 페이징 기법은 외부단편화가 발생하지 않아 메모리 낭비를 크게 줄였고, 압축 작업도 하지 않아도 된다. 대신, 조각난 프로세스들을 매핑해주고 차례대로 실행될 수 있도록 관리해주는 테이블이 필요하다.

### Paging(페이징)
![Paging](https://t1.daumcdn.net/cfile/tistory/27649A47590818AA2D)

- 하나의 프로세스가 사용하는 메모리 공간이 연속적이어야 한다는 제약을 없애는 메모리 관리 방법
- 외부단편화와 압축작업을 해소하기 위해 생긴 방법
- 프로세스를 일정 크기인 페이지로 잘라서 메모리에 적재하는 방식 : `고정분할`
    - 논리 주소를 같은 크기로 잘라(**페이지**) 페이지 테이블을 이용하여 물리 주소(**프레임**)에 할당함
    - 하나의 프로세스가 사용하는 공간은 논리 메모리에서 여러 개의 페이지로 나뉘어서 관리되고, 개별 페이지는 **순서에 상관없이** 물리 메모리에 있는 프레임에 mapping되어 저장된다
    - 즉, 서로 다른 위치의 프레임에 페이지들을 저장함
- 물리 메모리는 `Frame`이라는 고정 크기로 분리되어 있고, 논리 메모리는 `Page`라 불리는 고정 크기의 블록으로 분리된다.
    - Frame(프레임) : 물리 메모리를 고정크기 사이즈로 나눈것
    - Page(페이지) : 논리 메모리를 고정크기 사이즈로 나눈것
    - 프레임 사이즈 = 페이지 사이즈 (32bit 체제에서 4kb)
    - 논리 주소는 페이지 번호(p)와 변위(오프셋)(d), 물리 주소는 프레임 번호(f)+변위(d)로 표현된다.
- 프로세스마다 `Page Table`, `TLB`을 갖고 있다.

#### 장점

- 논리 메모리는 물리 메모리에 저장될 때 연속되어 저장될 필요가 없다. 즉, 물리 메모리의 남는 프레임에 적절히 배치됨으로 **외부 단편화를 해결**할 수 있다.
- 각 프로세스의 주소공간을 일부는 백킹스토어(스왑 영역)에 일부는 물리적 메모리에 혼재시키는 것이 가능하다.

#### 단점

- 할당이 항상 프레임의 정수 배로 할당되기 때문에 역으로 **내부 단편화의 비중이 증가**한다.
    
    > ex)  
    페이지 크기가 1,024B이고 Process A가 3,172B의 메모리를 요구한다면. 
    총 4개의 페이지 프레임( 3개의 페이지 프레임(1,024 * 3 = 3,072) + 100B )이 필요하다.  
    ⇒ 4번쩨 페이지 프레임에는 924B(1,024 - 100)의 여유공간이 남게 되는 내부 단편화 문제가 발생한다.  
- 내부 단편화를 감소시키기 위해 페이지 크기를 줄이면, 페이지 테이블이 커지게 된다.
- page table이 메모리에 올라가있기 때문에 데이터나 명령문에 접근하기 위해서는 **2번의 메모리 access**가 일어난다.(페이지 테이블에 한번, 페이지 테이블을 통해 실제 메모리에 한번)
⇒ 성능 향상을 위해 TLB사용
- 주소 변환 절차가 연속할당 방식에 비해 복잡하다.

#### Page mapping table

> 페이징을 이용하면 선형적이지 않으므로 **흩어진 프로세스들을 연결해줄 mapping table이 필요**

- 페이지와 프레임의 주소를 관리한다.
- page table의 인덱스 수는 page의 개수와 같다.
- 배열처럼 인덱스가 페이지 번호를 가르키고 그 배열에 담고있는 숫자가 매핑할 프레임 번호가 된다.
- 32비트 운영체제에서 페이지 크기가 4kb라 가정하면, 하나의 프로세스는 최대 2^20개의 페이지를 갖는다.
    - 페이지 오프셋이 12bits이므로, `2^32(운영체제) / 2^12(오프셋) = 2^20` 이다. 따라서 페이지는 20개 이상 존재할 수 없으므로, 하나의 프로세스는 최대 20개 페이지를 가질 수 있다.
- page번호는 Integer이므로 4byte! 페이지 테이블엔 2^20개의 페이지 정보가 들어있으므로 1MB. 즉 나의 페이지테이블의 크기는 `1MB*4byte = 4MB`
- 테이블은 용량이 크므로 CPU의 MMU에 넣을순 없으니(비용문제) RAM에 넣는다.
⇒ 메모리에 한 번 접근하기 위해 메모리를 2번 접근하는 비효율이생김. 이 성능문제로 인해 TLB가 나옴

#### TLB (translation look-aside buffer)

- 배경
    - 페이지 테이블을 사용하면 메모리에서 프레임번호를 확인하고, 다시 메모리에 접근해야 함
    - 페이지 테이블이 2^20개나 있지만 실제로는 거의 다 안쓰고 쓰는 영역만 쓴다는점에 초점을 맞춤
    - 캐시메모리에 자주 사용하는 page 몇 개에 대한 정보만 저장하여 성능 향상!
    - 이때 사용하는 캐시가 바로 TLB
- 속도향상을 위해 고속 캐시(하드웨어)인 TLB를 사용함
- TLB 전체 병렬 검색 후, TLB에 없으면 페이지 테이블에서 주소 찾음

### 가상 메모리 페이징

- 단순 페이징과 비교해서 프로세스 페이지 전부를 로드시킬 필요가없음
- 필요한 페이지가 있으면 후에 자동적으로 불러들어짐
- 장점
    - 외부단편화가없음
    - 다중 프로그래밍 정도가 높으며 가상 주소 공간이 큼
- 단점
    - 복잡한 메모리 관리 오버헤드

<br>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)

<br>

## 메모리 관리 전략3 (Segmentation)

### 세그멘테이션 (Segmentation)

![Segmentation](https://user-images.githubusercontent.com/34755287/57119448-47043400-6da5-11e9-95da-91cb808de992.png)

- 논리 메모리와 물리 메모리를 물리적인 단위인 페이지 말고 서로 다른 크기의 논리적 단위인 세그먼트(Segment)로 분할하는 방법

  > 돼지를 잡아서 보관을 한다고 생각해보자. 페이징의 방법을 사용하면 돼지를 모두 같은 단위로 잘라서 보관을 하는 것이다. 반면에 세그먼테이션은 부위별로 다른 크기로 잘라서 보관하는 것이다.

- `코드 & 데이터 & 스택`으로 구성하는 것도 이에 해당된다.

- 세그멘테이션은 집합별로 크기를 다르게 해서 자르므로 각 세그먼트의 크기는 일정하지 않다.

  - 그렇기 때문에 메모리를 미리 분할해둘 수 없고 메모리에 적재될 때 빈 공간을 찾아 할당한다.

- 세그멘테이션 기법 하의 논리주소는 `세그먼트 번호 s`와 `세그먼트 내 오프셋 d`로 이루어진다.

  - `세그먼트 번호 s` : 해당 논리 주소가 프로세스 주소 공간 내에서 몇 번째 세그먼트에 속하는지
  - `오프셋 d` : 그 세그먼트 내에서 얼마만큼 떨어져 있는지 

#### 세그먼트 테이블(Segment Table)

- 주소 변환을 위해 세그먼트 테이블(Segment Table)을 사용한다. 
- 세그먼트 테이블에는 세그먼트 번호, 시작주소(base), 세그먼트 크기(limit)를 저장한다.
  - `시작주소(base)` : 물리 메모리에서 세그먼트의 시작 위치
  - `세그먼트 크기(limit)` : 해당 세그먼트의 길이 (길이가 균등하지 않기 때문)
- 물리 주소 a는 base[s] + d로 계산된다.
- 세그먼트 크기를 넘어서는 주소가 들어오면 인터럽트가 발생해서 CPU가 해당 프로세스를 강제로 종료시킨다.

> 논리 주소 (2, 100) -> 물리 주소 4400번지
>
> 논리 주소 (1, 500) -> limit이 400이므로 인터럽트로 인해 프로세스 강제 종료 (범위 벗어남)

#### 보안

- 각 세그먼트별로 `protection bit`가 있다.
  - `valid bit = 0` : illegal segment
  - `Read/Write/Execution 권한 bit`

#### 공유

- 각 프로세스마다 공통된 세그먼트는 서로 공유할 수 있다.

#### 할당

- first fit / best fit이 효율적이다.

#### 장점과 단점

장점

- 내부 단편화 문제를 해소하기 위한 방법이다.
- 메모리 사용 효율이 개선되고 동적 분할을 통한 오버헤드가 감소한다.
- 의미 단위로 나뉘어져 있으므로 공유와 보안 측면에서 페이징보다 효과적이다.

단점

- 크기가 다른 각 세그먼트를 메무리에 두려면 동적 메모리 할당을 해야 한다.	
- 서로 다른 크기의 세그먼트들의 적재와 제거가 반복되면 자유 공간들이 많은 수의 작은 조각으로 나뉘어 사용하지 못하게 되기 때문에 외부 단편화 문제 해결이 불가능하다.

<br />

### 가상 메모리 세그멘테이션

- 필요하지 않은 세그먼트들을 로드되지 않는다.
- 장점
  - 높은 수준의 다중 프로그램
  - 큰 가상 주소 공간
  - 보호와 공유를 지원한다.
- 단점
  - 복잡한 메모리 관리로 인한 오버헤드가 존재한다.

<br />

### 페이지드 세그멘테이션 (Paged Segmentation)

- 프로그램을 위미 단위인 세그먼트로 나누고, 물리적 메모리에 적재하는 단위를 페이지 단위로 한다.
- 주소 변환을 위해 세그먼트 테이블(외부)와 페이지 테이블(내부) 두 단계 테이블을 사용한다.
- 메모리에 적재하면 페이징의 일정 단위로 다시 잘리기 때문에 외부 단편화가 발생하지 않는다.
- 테이블을 두 가지를 모두 거쳐야 하므로 속도 면에서 조금 떨어질 수 있다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)

<br />

## 가상메모리

### 가상 메모리 (배경, 가상 메모리가 하는 일, Demand Paging)


#### 가상 메모리란?

메모리를 관리하는 방법의 하나로, 실제 물리 메모리가 아닌 가상의 메모리를 말한다. 
멀티 태스킹 운영 체제에서 흔히 사용되며, 실제 메인 메모리의 용량보다 큰 영역을 제공한다.
실제 필요한 메모리는 MMU가 mapping한다.

#### 배경

과거 저장 매체는 아주 작은 용량을 가졌다. 
실제 메모리보다 큰 용량을 갖는 프로그램을 실행하면 메모리 부족 오류가 날 것이다. 
즉, 메모리 용량은 시스템 내의 최대 크기의 프로그램보다 커야한다. 
이런 제한 사항으로 인해 엔지니어들은 프로그램을 실행하는데 필요한 최소한의 메모리에 중점을 두고 가상 메모리를 개발했다. 
메모리 접근은 지역성으로 인해, 10000 바이트의 프로그램을 실행하는데 실제로 필요한 용량은 10000 바이트 보다 적다. 
결국, 실제 프로그램을 실행하는데 필요한 데이터만 메모리에 할당한다면 프로그램 전체 크기의 공간이 필요치 않다. 
실행에 필수적인 데이터 이 외에 나머지는 백킹 스토어에 위치하고 이것을 가상 메모리 기법이라 한다.

#### 프로그램의 일부만 메모리에 올릴 수 있다면...

- 물리 메모리 크기에 제약받지 않게 된다.
- 더 많은 프로그램을 동시에 실행할 수 있게 된다.
  - 이에 따라 `응답시간(Response Time)`은 유지되고, `CPU 이용률`과 `처리율(Throughput)`은 높아진다.
- [swap](../#swapping)에 필요한 입출력이 줄어들기 때문에 프로그램들이 빠르게 실행된다.

#### 가상 메모리 특징

- 메모리의 확장성 부여

  물리적 메모리는 한정적이지만, 가상 메모리는 훨씬 큰 공간으로 구성가능하다.

- 모든 프로그램에 동일한 메모리공간 제공

  개발자 입장에서 물리적 메모리가 아닌 OS가 제공하는 메모리 공간만 고려하면 된다. 실제 메모리 할당은 운영체제가 수행할 것이다.

- 주기억장치의 효율적 관리

   주기억장치를 하드디스크에 대한 캐시로 설정하여, 당장 사용하는 영역만 유지하고, 필요할 때만 램에 데이터를 올리고 사용하지 않으면 하드디스크로 내림으로써 램을 효과적으로 관리한다.

- 메모리 할당과 관리의 효율성

  물리적으로 연속되지 않은 메모리 공간이라도 가상으로 연속적인 메모리 공간으로 사용가능하다

- 메모리 용량 및 안정성 보장

  한정된 공간의 램이 아닌 거의 무한한 가상메모리 공간을 배정함으로써 프로세스들끼리 메모리 침범이 일어날 여지를 크게 줄인다.

- 하나의 프로세스만 실행 가능한 시스템(배치 처리 시스템 등)의 경우 가상 메모리를 필요로 하지 않는다.


#### 가상 메모리를 통한 라이브러리의 공유
논리적 메모리와 물리적 메모리를 분리하였기 때문에, 가상 메모리는 둘 이상의 프로세스가 파일 또는 메모리를 공유하게 해 준다. 

가상 주소 공간에 공유되는 객체를 매핑함으로써 시스템 라이브러리는 여러 프로세스에 공유될 수 있다. 각 프로세스는 공유되는 라이브러리를 자신의 공유 주소 공간의 한 부분으로 생각하지만, 실제로는 여러 프로세스들이 라이브러리가 존재하는 페이지들을 공유하는 것이다.

### Demand Paging

![요구 페이징](https://t1.daumcdn.net/cfile/tistory/2513314A55FE533722)

- 프로그램 실행 시작 시에 프로그램 전체를 디스크에서 물리 메모리에 적재하는 대신, 초기에 필요한 것들만 적재하는 전략
- 가상 메모리 시스템에서 많이 사용되며 가상 메모리는 대개 페이지로 관리된다.
- 요구 페이징을 사용하는 가상 메모리에서는 실행 과정에서 필요해질때 페이지들이 적재된다.
- 한 번도 접근되지 않는 페이지는 물리 메모리에 적재되지 않는다.
- 프로세스 내의 개별 페이지들은 `페이저(pager)`에 의해 관리된다.
  - 페이저는 프로세스 실행에 실제 필요한 페이지들만 메모리로 읽어 옴으로써 사용되지 않을 페이지를 가져오는 시간 낭비와 메모리 낭비를 줄일 수 있다.
- 이때 페이지 테이블은 메인 메모리에 적재되어 있는지 backing store에 있는지를 구분해주는것이 valid 비트이다


- valid - invalid bit
  - 페이지 테이블의 bit가 valid면 실제 메모리에 해당 페이지가 올라가 있다.
  - 백킹 스토어에 내려가 있으면 invalid이다.

- Page fault
  - invalid page에 접근하면 MMU가 page fault trap을 발생시킨다.
  - 커널 모드로 전환되고 page fault handler가 호출 된다.
  - Page fault 실행과정
    1. invalid라면 커널 모드로 전환되어 프로세스를 운영체제가 점유한다.
    2. 빈 페이지 프레임을 찾는다. 없다면 페이지 프레임을 교체한다.
    3. 해당 페이지를 백킹스토어(디스크)에서 메모리로 읽어온다.
       1. 디스크 IO가 끝날 때 까지 cpu는 다른 프로세스로 넘어간다.
       2. IO가 끝나면 메모리에 페이지를 올리고 page table의 bit를 바꾼다.
       3. 레디 큐에 프로세스를 넣는다.
    4. 이 프로세스가 cpu를 점유하고 running 상태로 바뀐다.
    5. 중단되었던 instruction을 재개한다.
- Performance
  - Page fault rate P = (0, 1). 0에 가까울 수록 page fault가 나지 않는다.
  - Effective Access time = (1 - P) * memory access + P * (OS & HW page fault overhead+ 페이지 스왑 인(필요하다면 스왑 아웃까지) + OS & HW restart overhead)

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)
<br/>

## 페이지 교체 알고리즘

### 페이지 교체
요구 페이징에서 언급된대로 프로그램 실행 시에 모든 항목이 물리 메모리에 올라오지 않기 때문에, 
프로세스의 동작에 필요한 페이지를 요청하는 과정에서 page fault (페이지 부재)가 발생하게 되면, 
원하는 페이지를 보조저장장치에서 가져오게 된다. 하지만 만약 물리 메모리가 모두 사용중인 상황이라면, 
페이지 교체가 이뤄져야 한다. (또는, 운영체제가 프로세스를 강제로 종료하는 방법이 있다.)
<br />

### FIFO
![FIFO](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FVQCGK%2FbtquJuqRkyS%2FLb3NgwHkBve08YhZpLkq31%2Fimg.png)

가장 간단한 페이지 교체 알고리즘으로 FIFO(first-in-first-out)의 흐름을 가진다.
먼저 물리 메모리에 들어온 페이지 순서대로 페이지 교체 시점에 먼저 나가게 된다.

- 장점
  - 이해하기 쉽고, 프로그래밍하기도 쉽다.

- 단점
  - 오래된 페이지가 항상 불필요하지 않은 정보를 포함하지 않을 수 있다. (초기 변수 등)
  - 처음부터 활발하게 사용되는 페이지를 교체해서 페이지 부재율을 높이는 부작용을 초래할 수 있다.
  - Belady의 모순 : 페이지를 저장할 수 있는 페이지의 프레임 개수를 늘려도 되려 페이지 부재가 더 많이 발생하는 모순이 존재한다.
<br />

### LRU (Least Recently Used)

![최적](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FSvRs7%2FbtquHbeJLQX%2FWXmK7xdGUbIxl43t0JG6Qk%2Fimg.png)

- 최적 알고리즘의 근사 알고리즘
- 대체적으로 FIFO 알고리즘보다 우수하고 OPT 알고리즘보다는 그렇지 못한 모습을 보인다.
- 가장 오랫동안 사용되지 않은 페이지를 선택하여 교체한다.
- 막대한 오버헤드 발생
<br />

### LFU (Least Frequently Used)
- 참조 횟수가 가장 적은 페이지를 교체하는 방법
- 가장 최근에 옮겨진 페이지가 교체될 가능성이 높음
- 초기에 많이 사용된후 더이상 사용되지 않는 페이지는 교체가능성 낮음
<br />

### NUR (Not Used Recently)
- LRU와 비슷, 최근에 사용하지 않은 페이지 교체
- LRU에서 나타나는 오버헤드를 줄일 수 있다.
- 최근의 사용여부를 확인 하기 위해 각페이지마다 두개의 비트 사용
<br />

### 최적 페이지 교체 (Optimal Page Replacement)
<img src="https://user-images.githubusercontent.com/80962918/136782651-cad45252-c7af-40e2-9538-6543784ff949.png" style width = "700"/>
미래에 사용되는 page를 미리 알고 있다고 가정할 때 구현 가능하다. 가장 먼 미래에 참조되는 페이지를 교체한다.

- 장점
  - 알고리즘 중 가장 낮은 페이지 부재율을 보장한다.
- 단점
  - 구현의 어려움이 있다. 모든 프로세스의 메모리 참조의 계획을 미리 파악할 방법이 없기 때문이다.
<br />

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)
<br />

## 메모리의 구조
<img src="https://i.imgur.com/fbzJjII.png" width="600" height="400">

- 프로그램이 실행되기 위해서는 먼저 프로그램이 메모리에 로드(load)되어야 하며, 프로그램에서 사용되는 변수들을 저장할 메모리도 필요하다.
- 따라서 컴퓨터의 운영체제는 프로그램의 실행을 위해 다양한 메모리 공간을 제공한다.
- 할당해주는 메모리 공간은 (Code,Data,Stack,Heap) 으로 이루어져 있다.

![메모리 공간](https://i.imgur.com/FdJ8Xbd.png)

### 코드(code) 영역
- 메모리의 코드(code) 영역은 실행할 프로그램의 코드가 저장되는 영역으로 텍스트 영역이라고도 부른다.
- 코드영역은 실행 파일을 구성하는 명령어들이 올라가는 메모리 영역으로 함수, 제어문, 상수 등이 여기에 지정된다.
- CPU는 코드 영역에 저장된 명령어를 하나씩 가져가서 처리한다.

### 데이터(data) 영역
- 프로그램의 전역 변수와 정적(static) 변수가 저장되는 영역이다.
- 데이터 영역은 프로그램의 시작과 함께 할당되며, 프로그램이 종료되면 소멸한다.
- 함수 내부에 선언된 static 변수는 프로그램이 실행될 때 공간만 할당되고 함수 실행시 초기화된다.

### 스택(stack) 영역
- 프로그램이 자동으로 사용하는 임시 메모리 영역
- 함수의 호출과 함께 할당되며, 함수의 호출이 완료되면 소멸한다.
- 함수 호출시 생성되는 local변수와 parameter가 저장되고 함수 호출이 완료되면 사라짐
- 스택 영역에 저장되는 함수의 호출 정보를 스택 프레임(stack frame)이라고 하며, 프로그램에서 현재 실행 중인 함수에 대한 정보를 저장하는 스택 자료구조를 Call Stack이라고도 한다.
- push로 데이터를 저장하고 pop으로 데이터를 꺼낸다.
- 스택은 후입선출(LIFO, Last-In First-Out) 방식에 따라 동작하므로, 가장 늦게 저장된 데이터가 가장 먼저 인출된다.
- 다른 메모리 영역과 다르게 높은 메모리 주소에서 낮은 메모리 주소의 방향으로 할당된다.
	- Stack은 서브루틴이 시작되면서 메모리 공간이 필요해진 지역변수들을 적재하고, 서브루틴이 끝나면 소멸한다. 그렇기때문에 데이터 용량에 대한 불확실성을 가지기 때문에 밑에서부터 채워 올린다.
- 컴파일 시 크기가 결정된다.

### 힙(heap) 영역
- 힙(heap) 영역은 사용자가 직접 관리할 수 있고 사용자가 관리해야하는 메모리 영역이다. (C에서는 프로그래머가 JAVA는 가비지컬렉터가 해제)
- 이 공간에 메모리를 할당하는 것을 동적 할당(Dynamic Memory Allocation)이라고 한다.  (예. new 연산자로 생성, class, 참조변수 등)
- 스택에서 포인터 변수를 할당하고, 그 포인터가 가리키는 힙 영역의 임의의 공간부터 동적 할당한 크기만큼 할당하여 사용한다.
- 메모리의 낮은 주소에서 높은 주소의 방향으로 할당된다.
- 런타임에 크기가 결정된다.


### 스택과 힙의 주소 할당 방향
<img src="https://user-images.githubusercontent.com/37038119/135815656-9829b1f6-310c-43be-a90a-6b789dc15a23.png"/>

> HEAP과 SATCK은 같은 공간을 공유하고 HEAP은 메모리 위쪽주소부터 할당되고 STACK은 아래쪽부터 할당된다</br>
<br/>

### 오버플로우
<img src="https://user-images.githubusercontent.com/37038119/135815562-c71cc5b8-cf40-4353-8b04-85263da5b68b.png" width="400" height="400"/>

> 말 그대로, 한정된 메모리 공간이 부족하여 메모리 안에 있는 데이터가 넘쳐 흐르는 현상. <br/>
> 오버 플로우의 종류 중에 힙 오버 플로우와 스택 오버 플로우가 있다. <br/>
> 힙은 메모리 위쪽 주소부터 할당되고, 스택은 메모리 아래쪽 주소부터 할당되기 때문에 각 영역이 상대 공간을 침범하는 일이 발생할 수 있다. <br/>
> 이때 힙이 스택을 침범하는 경우를 힙 오버 플로우라 하고, 스택이 힙을 침범하는 경우를 스택 오버 플로우라고 한다. <br/>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)


## 캐시의 지역성 (Locality, Caching line)

### 캐시의 지역성

캐시 메모리는 속도가 빠른 장치와 느린 장치 간의 속도차에 따른 병목 현상을 줄이기 위한 범용 메모리이다. 이러한 역할을 수행하기 위해서는 CPU가 어떤 데이터를 원할 것인가를 어느 정도 예측할 수 있어야 한다. 캐시의 성능은 작은 용량의 캐시 메모리에 CPU가 이후에 참조할, 쓸모 있는 정보가 어느 정도 들어있느냐에 따라 좌우되기 때문이다. 이 때 `적중률(Hit Rate)`을 극대화시키기 위해 `지역성(Locality)의 원리`를 사용한다.

> **지역성이란**
>
> 데이터 접근이 시간적, 혹은 공간적으로 가깝게 일어나는 것을 의미한다.
> 지역성의 전제조건으로 프로그램은 모든 코드나 데이터를 균등하게 Access하지 않는다는 특성을 기본으로 한다.
> 기억장치 내의 정보를 균일하게 Access하는 것이 아닌 어느 한 순간에 특정 부분을 집중적으로 참조하는 특성이다.

> **적중률 Hit ratio**
>
> 캐시 메모리 적중 횟수 / 전체 메모리 참조 횟수. 실패율은 1 - 적중율이다.


- 시간적 지역성
	- 특정 데이터가 한번 접근되었을 경우, 가까운 미래에 다시 접근될 확률이 높은 것을 말한다.
	- 따라서 캐시는 반복으로 사용되는 데이터가 많을수록 높은 효율을 보일 수 있다.
	- 메모리 상의 같은 주소에 여러 차례 읽기 쓰기를 수행할 경우, 상대적으로 작은 크기의 캐시를 사용해도 효율성을 꾀할 수 있다.

  e.g LRU, 공통 변수

- 공간적 지역성
	- 특정 데이터와 가까운 주소가 순서대로 접근되는 경우를 말한다. 
		-  즉, 하나의 기억장소가 참조되면 근처의 기억장소가 계속 참조될 가능성이 높음
	- 이때 특정 데이터 뿐만 아니라 주변 블럭 전체를 캐싱하여 성능을 높힌다. 
	- CPU 캐시나 디스크 캐시의 경우 한 메모리 주소에 접근할 때 그 주소뿐 아니라 해당 블록을 전부 캐시에 가져오게 된다.
	- 이때 메모리 주소를 오름차순이나 내림차순으로 접근한다면, 캐시에 이미 저장된 같은 블록의 데이터를 접근하게 되므로 캐시의 효율성이 크게 향상된다.
	
  e.g for 문과 같은 순차적 실행, 배열

- 순차적 지역성

  데이터가 순차적으로 접근되는 경향으로, 명령어들은 저장된 순서대로 인출되어 실행된다. 공간적 지역성에 포함되어 설명하기도 한다.

	
### Caching line

- 캐시(cache)는 프로세서 가까이에 위치하면서 빈번하게 사용되는 데이터를 놔두는 장소이다. 
캐시 메모리는 메인 메모리에 비해 저장 공간이 매우 작다. 따라서 메인 메모리의 데이터를 캐시 메모리에 1대1 대응 시킬 수 없다. 또한 캐시 메모리의 데이터 위치를 특정하지 못하면 캐시 메모리 전체를 순회해야하기 때문에 성능이 나빠진다. 따라서 캐시에 데이터를 저장 할때는 태그의 묶음으로 저장하게 되는데 이를 Caching line이라고 하며, 메모리로부터 가져올때도 캐싱 라인을 기준으로 가져온다.
> 태그는 정확한 캐시 메모리 주소를 알기 위한 정보이며 인덱스를 결합해서 메인 메모리 주소를 찾을수 있다

### 캐싱 라인의 종류

1. Direct mapping

	- 직접 매핑으로, 메인 메모리를 일정한 크기의 블록으로 나눠 캐시의 정해진 위치에 블록 단위로 저장한다.
	- 각 메모리 블록이 캐시의 딱 한곳에만 들어갈 수 있다.
	- 장점
		- 구현이 쉽고 비용이 적다.
		- 캐시 메모리에 데이터가 있는지 찾기 위해 비교는 한번만 해도 되어서 빠르다.
	- 단점
		- 적중률이 낮아질 수 있다.
		- 동일한 인덱스 필드 값을 가진 다수의 메모리 주소를 함께 저장할 수 없다. (address conflict)

![img](https://blog.kakaocdn.net/dn/KWMR2/btrgjyIn1ns/mvER1WQu1w7kI4BuHbQCqk/img.png)

2. Full associative mapping

	- 태그와 관계없이 비어있는 캐시 메모리를 탐색해서 집어넣고 데이터가 있는지 찾을때도 순차적으로 탐색해서 가져오는 방식이다.
	- 장점
		- 저장이 쉽다.
		- 충돌의 위험이 적다
	- 단점
		- 데이터에 특별한 순서가 없어 비교할 때마다 순차탐색 시간이 발생하므로 오래 걸린다.

3.  Set associative mapping

	- Direct Mapping과 Full Associative Mapping의 장점만을 취한 방식.
		-  Direct Mapping에서 동일 인덱스 필드의 메모리 영역 데이터를 함께 공유할 수 없다는 단점을 극복
	- 테이블을 여러 개 만들면 같은 태그의 메모리라도 다른 테이블의 태그에 저장하면 되는 원리라서 테이블의 개수에 따라 `n-way set associative`라고 부른다.
	- n번만큼 탐색해서 `Direct Map`보다는 시간이 더 걸리겠지만 충돌 위험은 줄어든다.
	- 특정 row 를 지정하여 어떤 열이라도 비어있다면 저장.
	- DMC에 비해 검색은 오래 걸리지만 저장이 빠르며 Associative에 비해 저장이 느리지만 검색이 빠른 중간형.


#### 저장속도 : Full Associative > Set Associative > Direct

#### 검색속도 : Direct > Set Asoociative > Full Associative

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)

## 교착상태 (Dead Lock)

두 개 이상의 작업이 서로 상대방의 작업이 끝나기 만을 기다리고 있기 때문에 아무 것도 완료되지 못하는 상태를 가르킨다.

### 교착상태의 발생조건

교착상태가 발생할 조건은 아래 4가지 조건을 모두 만족해야 한다. 반대로 말하면 4가지 조건 중 하나라도 만족하지 않으면 교착상태는 발생하지 않는다.

- 상호 배제
  - 한 번에 프로세스 하나만 해당 자원을 독점적으로 사용한다. 사용 중인 자원을 다른 프로세스가 사용하려면 요청한 자원이 해제될 때까지 기다려야 한다.
- 점유 대기
  - 자원을 최소한 하나 보유하고, 다른 프로세스에 할당된 자원을 점유하기 위해 대기하는 프로세스가 존재해야 한다.
- 비선점
  - 이미 할당된 자원을 강제로 빼앗을 수 없다.
- 순환 대기
  - 대기 프로세스의 집합이 순환 형태로 자원을 대기하고 있어야 한다.

### 교착상태의 해결법

- 데드락이 발생하지 않도록 `예방(prevention)`하기
- 데드락 발생 가능성을 인정하면서도 적절하게 `회피(avoidance)` 하기
- 데드락 발생을 허용하지만 데드락을 `탐지(detection)`하며 데드락에서 `회복(recovery)` 하기
- 교착상태 발생을 무시하기

**교착상태를 미리 예방**

- 자원 할당 시 4가지 조건 중 어느 하나가 만족되지 않도록 한다.

  1. 상호 배제 : 공유해서는 안되는 자원의 경우 상호배제를 피할 수 없다. 보통 나머지 조건들을 통해 예방한다.

  2. 점유 대기: 프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 한다.

     e.g 1. 프로세스 시작 시 필요한 모든 자원을 할당 받는다.

     e.g 2. 자원이 필요할 경우 보유 자원을 반납하고 다시 요청한다.

  3. 비선점: 프로세스가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선점된다.(보유 자원을 뺏긴다. 보유 자원은 save, restore가 쉬워야 한다.)

  4. 순환 대기: 모든 자원에 할당 순서를 정하여 정해진 순서대로만 자원 할당

     e.g 순서가 3인 자원 Ri를 보유한 프로세스가 순서가 1인 자원 Rj를 할당 받기 위해서는 우선 Ri를 반납해야 한다.

- 문제점: 자원 이용률 저하, 시스템 처리율 감소, 기아 문제

**교착상태 발생을 회피**

- 자원 요청에 대한 부가적인 정보를 이용해서 교착상태의 가능성이 없는 경우에만 자원을 할당한다.
- 시스템 state가 원래 state로 돌아올 수 있는 경우에만 자원을 할당한다.
- [은행원 알고리즘](https://github.com/GumiMobile/CS-Study/blob/main/OS/Problem%20List/교착상태.md#은행원-알고리즘)

**교착상태 탐지 및 회복**

- **탐지 기법**
  - Allocation, Request, Available 등으로 시스템에 교착 상태가 발생했는지 여부를 탐색한다.
  - 즉, 은행원 알고리즘에서 했던 방식과 유사하게 현재 시스템의 자원 할당 상태를 가지고 파악한다.
  - 이외에도 자원 할당 그래프를 통해 탐지하는 방법도 있다.
- **회복 기법**
  - 교착 상태를 탐지 기법을 통해 발견했다면, `순환 대기`에서 벗어나 교착 상태로부터 회복하기 위한 방법을 사용한다.
  - 단순히 프로세스를 1개 이상 중단
    - 교착 상태에 빠진 모든 프로세스를 중단시키는 방법
      - 계속 연산 중이던 프로세스들도 모두 일시에 중단되어 부분 결과가 폐기될 수 있는 부작용이 발생할 수 있다.
    - 프로세스를 하나씩 중단시킬 때마다 탐지 알고리즘으로 교착 상태를 탐지하면서 회복시키는 방법
      - 매번 탐지 알고리즘을 호출 및 수행해야 하므로 부담이 되는 작업일 수 있다.
  - 자원 선점
    - 프로세스에 할당된 자원을 선점해서, 교착 상태를 해결할 때까지 그 자원을 다른 프로세스에 할당해주는 방법

**교착상태 무시**
- 교착상태를 시스템이 책임지지 않는다.
- UNIX를 포함한 대부분의 OS가 채택

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#operating-system)
