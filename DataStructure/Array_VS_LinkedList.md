## Array VS LinkedList
### 이유진

Array는 데이터의 논리적 저장 순서와 물리적 저장 순서가 일치하여 인덱스를 통해 데이터에 접근할 수 있다. LinkedList는 각각의 데이터들이 자신의 이전과 이후에 오는 데이터를 기억하고 있다.

Array는 데이터가 연속적으로 저장되어있어 인덱스를 통해 데이터에 한 번에 접근할 수 있다. 따라서 검색 속도가 빠르지만, 삽입, 삭제시 O(n)의 시간복잡도가 발생하고 크기 변경이 불가능하다.
LinkedList는 현재 데이터의 이전,다음 데이터가 무엇인지 기억한다. 따라서 삽입, 삭제를 O(1)만에 할 수 있다. 대신 검색 시 O(n)의 시간복잡도를 갖게 된다.

### 우지현

|                              |                            Array                             |                          LinkedList                          |
| :--------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|             구조             |           논리적 저장 순서와 물리적 저장 순서 일치           | 자료의 주소값으로 노드를 이용해 서로 연결되어 있는 구조<br />Tree의 근간이 되는 자료구조 |
| 요소 접근 <br />(탐색, 조회) | Random Access<br />인덱스를 통해 원소에 접근 가능<br />시간 복잡도 O(1) | Sequantial Access<br />순차적으로 접근하면서 찾아야 함<br />시간 복잡도 O(n) |
|             크기             |                            제한적                            |                    동적으로 크기가 늘어남                    |
|          삽입, 삭제          | 시간 복잡도 O(n)<br />(해당 원소에 접근하여 작업 완료 후 shift 해주어야 함) | 처음이나 마지막 삽입, 삭제시 O(1)<br />중간에 삽입, 삭제시 O(n) |
|         메모리 할당          | Array가 선언되자 마자 Compile Time에 할당됨<br />정적 메모리 할당 (Static Memory Allocation) | 새로운 Node가 추가될 때 Runtime에 할당됨<br />동적 메모리 할당 (Dynamic Memory Allocation) |
|     메모리 할당 Section      |                            Stack                             |                             Heap                             |
|             종류             | Single Dimensional Array<br />Two Dimensional Array<br />Multidimensional Array | Linear(Singly) Linked List<br />Double Linked List<br />Circular Linked List |
|             결론             |            데이터에 접근하는 것이 중요할 때 사용             |                 삽입과 삭제가 빈번할 때 사용                 |

