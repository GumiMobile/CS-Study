# CPU 스케줄러

## 우지현

###  CPU 스케줄러 (CPU Scheduler)

스케줄러는 Ready Queue에 있는 프로세스들을 특정한 우선순위를 기반으로 CPU를 할당받게 해주는 역할을 한다. 

일반적인 시스템에서는 다음과 같은 목적을 공통적으로 지닌다.

- No Starvation(기아) : 각각의 프로세스들이 오랜 시간 동안 CPU를 할당받지 못하는 상황이 없도록 한다.
- Fairness : 각각의 프로세스에 공평하게 CPU를 할당해준다.
- Balance : Keeping all parts of the system busy

#### 스케줄링의 목표

- Batch System (일괄처리 시스템)
  - 온라인처럼 일에 대한 요청이 발생했을 때, 즉각적으로 처리하는 것이 아닌 일정기간 또는 일정량을 모아뒀다가 한번에 처리하는 방식
  - Throughput (처리량) : 시간당 최대의 작업량을 낸다.
  - Turnaround time : 프로세스의 생성부터 소멸까지의 시간을 최소화한다.
  - CPU utilization : CPU가 쉬는 시간이 없도록 한다.
- Interactive System (대화형 시스템)
  - 온라인과 같이 일에 대한 요청에 대해 즉각적으로 처리하여 응답을 받을 수 있는 시스템
  - Response time : 즉각적으로 처리해야하는 시스템이므로 요청에 대해 응답시간을 줄이는 게 중요하다.
- Time Sharing System
  - 각 프로세스에 CPU에 대한 일정시간을 할당하여 주어진 시간 동안 프로그램을 수행할 수 있게 하는 시스템
  - Meeting deadlines : 데이터의 손실을 피하며, 끝내야하는 시간 안에 도달해야 한다.
  - Predictability : 멀티미디어 시스템에서의 품질이 저하되는 부분을 방지해야 한다.

### 선점 vs 비선점 스케줄링

> 프로세스의 상태 전이
>
> ![프로세스 상태](https://t1.daumcdn.net/cfile/tistory/990DB03F5C7AC10303)
>
> - 승인 (Admitted) : 프로세스 생성이 가능하여 승인됨
> - 스케줄러 디스패치 (Scheduler Dispatch/Scheduled) : 준비 상태에 있는 프로세스 중 하나를 선택하여 실행시키는 것
> - 인터럽트 (Interrupt/Time-slice burst) : 예외, 입출력, 이벤트 등이 발생하여 현재 실행 중인 프로세스를 준비 상태로 바꾸고 해당 작업을 먼저 처리하는 것
> - 입출력 또는 이벤트 대기 (I/O or Event wait) : 실행 중인 프로세스가 입출력이나 이벤트를 처리해야 하는 경우, 입출력/이벤트가 모두 끝날 때까지 대기 상태로 만드는 것
> - 입출력 또는 이벤트 완료 (I/O or Event Completion) : 입출력/이벤트가 끝난 프로세스를 준비 상태로 전환하여 스케줄러에 의해 선택될 수 있도록 만드는 것

선점(preemptive) 스케줄링

![선점 스케줄링](https://t1.daumcdn.net/cfile/tistory/993815445C7AC10303)

- 낮은 우선순위를 가진 프로세스보다 높은 우선순위를 가진 프로세스가 CPU를 선점하는 방식
- OS가 스케줄링의 알고리즘에 따라 적당한 프로세스에게 CPU를 할당하고, 필요시에는 회수하는 방식
- 프로세스가 자율적으로 CPU를 반납하는 시점(`I/O or Event Wait`, `Exit`)에서 사용한다. 

비선점(non-preemptive) 스케줄링

![비선점 스케줄링](https://t1.daumcdn.net/cfile/tistory/998E6C405C7AC10302)

- Time-slice가 없는 스케줄링
- CPU를 사용중인 프로세스가 자율적으로 반납하도록 하는 방식
- 프로세스 종료  or I/O 등의 이벤트가 있을 때까지 실행을 보장하지만 처리시간 예측이 어렵다.
- 일반적으로 `Interrupt`, `I/O or Even Completion` 이 발생한 시점에서 사용하지만 상황에 따라 `I/O or Event Wait`, `Exit` 상황에서도 스케줄링이 가능하다.

### CPU 스케줄링 종류

비선점 스케줄링 (Non-preemptive Scheduling)

- FCFS (First Come First Served)

  - 먼저 온 고객을 먼저 서비스해주는 방식. 즉, 먼저 온 순서대로 처리.

  - 문제점

    - convoy effect

      소요시간이 긴 프로세스가 먼저 도달하여 효율성을 낮추는 현상이 발생한다.

- SJF (Shortest Job First)

  - 다른 프로세스가 먼저 도착했어도 CPU burst time이 짧은 프로세스에게 선 할당

  - 문제점

    - starvation

      효율성을 추구하는게 가장 중요하지만 특정 프로세스가 지나치게 차별받으면 안되는 것이다. 이 스케줄링은 극단적으로 CPU 사용이 짧은 job을 선호한다. 그래서 사용 시간이 긴 프로세스는 거의 영원히 CPU를 할당받을 수 없다.

- HRN (Highest Response-ratio Next)

  - 대기 중인 프로세스 중 현재 응답률(Response Ratio)이 가장 높은 것을 선택
  - SJF의 약점인 starvation을 보완한 기법으로 긴 작업과 짧은 작업 간의 불평등 완화
  - HRN의 우선순위 = (대기시간 + 서비스 시간) / 서비스 시간

- Priority Scheduling

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

      시스템에서 특정 프로세스의 우선순위가 낮아 무한정 기다리게 되는 경우, 한 번 양보하거나 기다린 시간에 비례하여 일정 시간이 지나면 우선순위를 한 단계씩 높여 가까운 시간 안에 자원을 할당받도록 하는 기법



선점 스케줄링 (Preemptive Scheduling)

- SRT (Shortest Remaining Time First)
  - 새로운 프로세스가 도착할 때마다 새로운 스케줄링이 이루어진다.
  - 문제점 
    - starvation
    - 새로운 프로세스가 도달할 때마다 스케줄링을 다시 하기 때문에 CPU burst time(CPU 사용시간)을 측정할 수가 없다. 
- RR (Round Robin)
  - 현대적인 CPU 스케줄링
  - 각 프로세스는 동일한 크기의 할당 시간(time quantum)을 갖게 된다.
  - 할당 시간이 지나면 프로세스는 선점당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다.
  - CPU 사용시간이 랜덤한 프로세스들이 섞여있을 경우에 효율적이다.
  - RR이 가능한 이유는 프로세스의 context를 save할 수 있기 때문이다.
  - 장점
    - Response time이 빨라진다.
    - n개의 프로세스가 ready queue에 있고 할당시간이 q(time quantum)인 경우 각 프로세스는 q 단위로 CPU 시간의 1/n을 얻는다. 즉, 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.
    - 프로세스가 기다리는 시간이 CPU를 사용할 만큼 증가한다. 공정한 스케줄링이라고 할 수 있다.
  - 주의할 점
    - 설정한 time quantum이 너무 커지면 FCFS와 같아진다. 또 너무 작아지면 스케줄링 알고리즘의 목적에는 이상적이지만 잦은 context switch로 overhead가 발생한다. 그렇기 때문에 적당한 time quantum을 설정하는 것이 중요하다.





## 김현수

### CPU 스케줄러란?
- 다중 프로그램 OS의 기본으로, 여러 프로세스들이 CPU를 교환하며 보다 생산적으로 동작한다.
- CPU를 선점한 프로세스가 대기하는 시간을 보다 효율적으로 사용하기 위하여 사용한다.
- 기본적으로 다음에 실행될 프로세스를 정하고, 프로세스들을 실행가능한 상태로 만든다.

### 스케줄링의 목적
- 공평성 : 모든 프로세스가 자원을 공평하게 배정받아야 하며, 특정 프로세스가 배제되어서는 안 된다.
- 효율성 : 시스템 자원을 놀리는 시간 없이 스케줄링해야 한다.
- 안정성 : 우선순위를 사용하여 중요한 프로세스가 먼저 처리되도록 해야 한다.
- 반응 시간 보장 : 응답이 없는 경우 사용자는 시스템이 멈춘 것으로 가정하기 때문에 시스템은 적절한 시간 안에 프로세스의 요구에 반응해야 한다.
- 무한 연기 방지 : 특정 프로세스의 작업이 무한히 연기되어서는 안 된다.

### 비선점 스케줄링과 선점 스케줄링
- 비선점 스케줄링 (Non-preemptive scheduling)
	- Time-slice 가 없는 스케줄링
	- CPU를 사용중인 프로세스가 자율적으로 반납하도록 하는 방식
	- 따라서 프로세스가 자율적으로 CPU를 반납하는 시점에서 사용한다.

- 선점 스케줄링 (Preemptive scheduling)
	- 낮은 우선순위를 가진 프로세스보다 높은 우선순위를 가진 프로세스가 CPU를 선점하는 방식
	- OS가 스케줄링의 알고리즘에 따라 적당한 프로세스에게 CPU를 할당하고, 필요시에는 회수하는 방식

### 스케줄링 알고리즘

#### 비선점 스케줄링 
- FCFS (First Come First Served)
	- 먼저 CPU를 요청하는 프로세스를 먼저 처리하는 방식
	- 큐 자료구조를 사용하여 구현한다.
	- CPU를 사용하고 있는 프로세스 자기 자신이 CPU를 놓아줄 때까지 CPU를 다른 프로세스에게 양보하지 않는다.
	- CPU를 매우 오래 사용하는 프로세스가 도착하게 되면, 다른 프로세스가 CPU를 사용하는데 기다리는 대기 시간이 매우 커진다.(콘보이 현상)
		- 평균 대기시간이 매우 길다.
	- 비선점 스케줄링 알고리즘은 모든 프로세스가 공평한 조건으로 CPU를 사용할 수 있게 해주며, 반응 시간을 예측할 수 있다.
	
- SJF (Shortest Job First)
	- 가장 시간이 적게 걸리는 프로세스를 선택하여 CPU를 할당한다.
	- 평균 waiting time 을 최소화하기 위해 사용하는 방식이다.
	- 늦게 도착하더라도 CPU 처리 시간이 앞에 대기중인 프로세스보다 짧으면 먼저 CPU를 할당받을 수 있기 때문에 콘보이 효과를 완화할 수 있다.
	- CPU 점유 시간 자체를 예측해야만 구현할 수 있다.
	- 실행 시간이이 긴 프로세스은 오랜 시간 굶주려야 하므로, No starvation을 어기게 된다.
	
#### 선점 스케줄링 
- SRT (Shortest Remaning Time )
	- SJF의 선점형 방식이다. 
	- 먼저 온 프로세스가 CPU를 할당받고 있더라도 남은 처리 시간이 뒤에 온 프로세스의 처리 시간보다 길면 CPU를 빼앗긴다.
	- 어떤 알고리즘보다 평균 대기 시간이 가장 짧다.
	- 선점형 방식이기 때문에 잦은 문맥교환이 일어나고 그에 따른 오버헤드가 커진다. 
	- starvation 현상이 더 심각하게 발생할 수 있다.
	- CPU의 예상 시간을 예측하기 힘들기 때문에 실제로 사용되기는 어렵다. 
	
- Priority Scheduling (우선 순위 스케줄링)
	- 우선 순위가 높은 프로세스에 CPU를 우선 할당하는 방식의 스케줄링
	- 우선 순위는 시간 제한, 메모리 요구량, 프로세스의 중요성, 자원사용 비용 등에 따라 달라질 수 있다.
	- 우선 순위가 같을 경우, FCFS와 다를게 없다. (비선점, 선점 둘다 사용된다.)
	- 우선순위가 낮은 프로세스의 대기 시간이 무한정 늘어날 수 있다.
		- 이를 해결하기 위해 Aging-method 사용 : 레디 큐에서 오랜 시간을 기다린 프로세스는 계속해서 우선순위를 높히는 기법
		
- RR(Round Robin)
	- 각각의 프로세스가 동일한 CPU 할당 시간(타임 슬라이스, quantum) 동안만 CPU를 이용하게 한다. 
		- 할당 시간동안 처리를 다 하지 못하면 CPU를 빼앗고 다음 프로세스에게 넘긴다. 
		- CPU를 빼앗긴 프로세스는 레디 큐의 맨 뒤로 간다.
	- CPU 처리 시간을 계산하지 않아도 돼서 선점형 방식의 가장 단순하고 대표적인 방법이다. 
	- 우선 순위도 없기 때문에 매우 공평하다.
	- 레디 큐에 n개의 프로세스와 q의 time qunatum을 가지고 있다면, 모든 프로세스는 최대 (n-1)q 시간 안에 CPU 사용을 시작할 수 있음을 보장받을 수 있다.
		- 콘베이 효과 완화
	- 타임 슬라이스의 크기 결정이 매우 중요하다.
		- 타임 슬라이스가 큰 경우: 처리 시간이 긴 프로세스에 의해 CPU의 효율성이 떨어질 수 있다. 만약 타임 슬라이스가 무한대로 설정되면 FCFS 스케줄링과 다를 바 없어진다. 
		- 타임 슬라이스가 작은 경우: 여러 프로그램이 동시에 실행되는 효과를 볼 수 있지만 너무 작으면 잦은 문맥 교환이 일어나 오버헤드가 상당히 커진다.
		- 보통 10-100 ms로 설정한다.


### 이수형

### CPU 스케줄러

다중 프로그램 OS의 기본으로, 여러 프로세스들이 CPU를 교환. CPU를 선점한 프로세스가 대기하는 시간을 보다 효율적으로 사용하기 위하여 사용

### 스케줄링 알고리즘

- FCFS
  - CPU를 먼저 요청하는 프로세스를 처리하는 방식
  - CPU를 요청하는 프로세스의 수행시간에 따라 평균 대기시간이 달라진다

- SJF
  - 평균 waiting time 을 최소화하기 위해 사용하는 방식
  - 수행시간이 짧은 프로세스부터 CPU할당
  - 수행시간이 긴 프로세스는 오랫동안 할당받지 못함

- SRT
  - 최단 잔여시간을 우선으로 하는 스케줄링.
  - 프로세스가 현재 진행중이여도 잔여시간이 더 짧은 프로세스가 있으면 그것을 먼저 처리
  - 그러나 예상시간을 예측하기 어렵기때문에 실 사용에는 어려움
- Priority Scheduling
  - 프로세서의 우선순위를 정해진 규칙에 따라 정한뒤 우선순위대로 처리
  - 우선순위가 같을 경우 FCFS과 똑같음

- RR
  - 모든 프로세스가 같은 우선순위를 가진고, time slice를 기반으로 스케줄링한다.
  - 공평하게 처리할 수 있으나 Time slice에 따라 성능이 좌우되며 너무크면 FCFS가되고 너무작으면 불필요한 전환이 많이 일어남


## 이유진
### CPU 스케줄러
준비 상태에 있는 프로세스들 중 어느 프로세스에게 CPU를 할당 할 지 결정하는 운영체제의 코드

### 스케줄링 알고리즘
#### 선입선출 (FCFS)
- 프로세스가 준비큐에 도착한 시간 순서대로 CPU를 할당하는 방식
- 먼저 요청한 프로세스에게 CPU를 먼저 할당하고, 그 프로세스가 자발적으로 CPU를 반납할 때까지 빼앗지 않는다.
- (단점) 평균 대기시간이 길어진다. 
- (단점) CPU버스트가 짧은 프로세스들이 CPU버스트를 마친 뒤 I/O작업을 수행할 수 있는데, CPU버스트가 긴 프로세스 하나를 대기하며 I/O장치들의 이용률까지도 하락할 수 있다.

#### 최단 작업 우선 (SJF)
- CPU 버스트가 가장 짧은 프로세스에게 제일 먼저 CPU를 할당하는 방식
- 평균 대기시간을 가장 짧게 하는 최적 알고리즘으로 알려져 있다.
- 비선점형/선점형 두 가지로 구현될 수 있다.
- SJF구현에서 어려운 부분은 프로세스의 CPU버스트 시간을 미리 알 수 없다는 점이다. 따라서 과거의 CPU버스트 시간을 통해 예측한다.
- (단점) CPU버스트가 긴 프로세스는 준비 큐에서 무한정 대기해야 할 수 있다. => **기아현상**

#### 최단 잔여시간 우선 (SRT)
- 가장 짧은 잔여시간을 우선으로 하는 방식
- 선점형 : 프로세스가 진행중이어도, 더 짧은 프로세스가 생기면 먼저 할당한다.

#### 우선순위 (Priority Scheduling)
- 준비큐에서 기다리는 프로세스 중 우선순위가 가장 높은 프로세스에게 제일 먼저 CPU를 할당하는 방식
- (단점) 기아현상이 발생할 수 있다. 
- (해결방안) **노화 기법** : 기다리는 시간이 길어지면 우선 순위를 조금씩 높여준다.

#### Round Robin
- 시스템의 성질을 가장 잘 활용한 방식
- 각 프로세스가 CPU를 연속적으로 사용할 수 있는 시간을 특정 시간으로 제한한다. 이 시간이 경과하면 CPU를 회수하여 다른 프로세스에게 CPU를 할당하고, 작업을 하던 프로세스는 준비큐에 가서 대기한다.
    - 타이머 인터럽트를 통해 CPU를 회수한다.
- 할당시간 : CPU를 연속적으로 사용할 수 있는 최대 시간
    - 할당시간이 너무 길면 FCFS와 같은 결과가 발생한다.
    - 할당시간이 너무 짧으면 CPU를 사용하는 프로세스가 빈번하게 교체되어 문맥교환의 오버헤드가 커진다.
    - 따라서 일반적으로 할당시간은 `수십ms` 정도의 규모로 설정한다.
- 일반적으로 SJF보다 평균 대기시간은 길지만 응답시간은 더 짧다. SJF보다 공정하다.
