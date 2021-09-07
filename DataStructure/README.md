# Data Structure

* [Array VS Linked List](#array-vs-linked-list)
  * [Array (배열)](#array-----)
  * [Linked List (연결리스트)](#linked-list--------)

[뒤로](https://github.com/GumiMobile/CS-Study)

</br>

## Array VS Linked List

### Array (배열)

> 데이터 접근(탐색, 조회)에 유리.

- 정적인 자료구조로, 컴파일 타임에 크기가 결정됨

- 선언 시에 크기를 지정해주며 메모리에 고정되기 때문에 크기 변경 불가

- 데이터의 논리적 저장 순서와 물리적 저장 순서가 일치함

- 인덱스를 통해 데이터에 빠른 접근 가능 O(1)

- 데이터의 삽입, 삭제 시 기존 데이터를 모두 뒤나 앞으로 이동시켜야 하기 때문에 O(N)

- 사용할 자료의 사이즈가 명확할 때, 데이터의 삽입/제거가 아주 빈번한 게 아니라면 Linked List보다 효율적임

  

### Linked List (연결리스트)

> 데이터 수정(삽입, 삭제)에 유리.

- 동적인 자료구조로, 런타임에 크기가 계속해서 변함

- 트리 구조의 근간이 되는 자료구조

- 각각의 데이터들이 자신의 이전, 다음 데이터가 무엇인지 기억함

- 데이터를 삽입할때마다 동적으로 크기 증가

- 순차접근을 해야하므로 배열에 비해 접근 속도가 느림 O(n)

- 인덱스를 갖지 않고 데이터의 위치를 기억하기 때문에 삽입, 삭제 시 O(1)

- 논리 구조상 Linked List가 Array보다 효율적인 경우가 많지만, 일반적으로 Array를 사용하는게 더 빠른 경우가 많다. 이는 Linked List가 동적으로 크기가 변하기 때문인데, 메모리를 할당하고 해제할 때 시스템 콜을 사용하기 때문이다. 시스템 콜은 시간복잡도에 포함되지 않지만 시간이 꽤 걸리는 작업이다.

  

위 내용을 표로 정리하면 다음과 같다.

|                              |                            Array                             |                         Linked List                          |
| :--------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|             구조             |           논리적 저장 순서와 물리적 저장 순서 일치           | 자료의 주소값으로 노드를 이용해 서로 연결되어 있는 구조<br />Tree의 근간이 되는 자료구조 |
| 요소 접근 <br />(탐색, 조회) | Random Access<br />인덱스를 통해 원소에 접근 가능<br />시간 복잡도 O(1) | Sequential Access<br />순차적으로 접근하면서 찾아야 함<br />시간 복잡도 O(n) |
|             크기             |                            제한적                            |                    동적으로 크기가 늘어남                    |
|          삽입, 삭제          | 시간 복잡도 O(n)<br />(해당 원소에 접근하여 작업 완료 후 shift 해주어야 함) | 처음이나 마지막 삽입, 삭제시 O(1)<br />중간에 삽입, 삭제시 O(n) |
|         메모리 할당          | Array가 선언되자 마자 Compile Time에 할당됨<br />정적 메모리 할당 (Static Memory Allocation) | 새로운 Node가 추가될 때 Runtime에 할당됨<br />동적 메모리 할당 (Dynamic Memory Allocation) |
|     메모리 할당 Section      |                            Stack                             |                             Heap                             |
|             종류             | Single Dimensional Array<br />Two Dimensional Array<br />Multidimensional Array | Linear(Singly) Linked List<br />Double Linked List<br />Circular Linked List |
|             결론             |            데이터에 접근하는 것이 중요할 때 사용             |                 삽입과 삭제가 빈번할 때 사용                 |



[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#Data-Structure)

</br>
