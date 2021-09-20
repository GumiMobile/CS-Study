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
