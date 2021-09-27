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





### 

