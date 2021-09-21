# Data Structure

* [Array VS Linked List](#array-vs-linked-list)
  * [Array (배열)](#array-배열)
  * [Linked List (연결리스트)](#linked-list-연결리스트)
* [Stack and Queue](#stack-and-queue)
  * [Stack](#stack)
    * [Stack의 활용](#stack의-활용)
  * [Queue](#queue)
    * [Queue의 활용](#queue의-활용)
* [Tree](#tree)
  * [Tree 관련 용어](#tree-관련-용어)
  * [Binary Tree (이진 트리)](#binary-tree-이진-트리)
  * [Full Binary Tree (정 이진 트리)](#full-binary-tree-정-이진-트리)
  * [Complete Binary Tree (완전 이진 트리 균형 트리)](#complete-binary-tree-완전-이진-트리-균형-트리)
  * [Perfect Binary Tree (포화 이진 트리 균형 트리)](#perfect-binary-tree-포화-이진-트리-균형-트리)
  * [Binary Search Tree (이진 탐색 트리)](#binary-search-tree-이진-탐색-트리)

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

<br />


### Linked List (연결리스트)

> 데이터 수정(삽입, 삭제)에 유리.

- 동적인 자료구조로, 런타임에 크기가 계속해서 변함
- 트리 구조의 근간이 되는 자료구조
- 각각의 데이터들이 자신의 이전, 다음 데이터가 무엇인지 기억함
- 데이터를 삽입할때마다 동적으로 크기 증가
- 순차접근을 해야하므로 배열에 비해 접근 속도가 느림 O(n)
- 인덱스를 갖지 않고 데이터의 위치를 기억하기 때문에 삽입, 삭제 시 O(1)
- 논리 구조상 Linked List가 Array보다 효율적인 경우가 많지만, 일반적으로 Array를 사용하는게 더 빠른 경우가 많다. 이는 Linked List가 동적으로 크기가 변하기 때문인데, 메모리를 할당하고 해제할 때 시스템 콜을 사용하기 때문이다. 시스템 콜은 시간복잡도에 포함되지 않지만 시간이 꽤 걸리는 작업이다.

<br />

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

<br /><br />

## Stack and Queue

### Stack

<img src="https://user-images.githubusercontent.com/37680108/133214006-337acdd0-c3ba-4d2d-a329-85f711701b7e.png"  width="500" style="float : left;"/>

- 데이터를 차곡차곡 쌓아 올린 형태의 선형 자료구조

- 후입선출 (Last In First Out, LIFO)

- 입구와 출구가 같다. 즉, 삽입과 삭제가 한 방향에서만 이루어진다.

  일반적으로 top이라는 하나의 공간을 통해 입출력이 일어난다. 

  - top은 가장 최근에 들어온 자료를 가리킨다.

  - push : top이 가리키는 자료의 위에 새 자료를 삽입한다.

    > 🚨 이미 가득 차 있는 스택에 자료를 넣으려고 하는 경우 stack overflow가 발생한다. 

  - pop : top이 가리키고 있는 자료를 반환하고 삭제한다.

    > 🚨  빈 스택에서 자료를 추출하려 하는 경우 stack underflow가 발생한다.
    

<br />

#### Stack의 활용

- 웹 브로우저 방문기록 (뒤로 가기): 가장 나중에 열린 페이지부터 다시 보여준다.
- 실행 취소 (undo) : 가장 나중에 실행된 것부터 실행을 취소한다.
- 후위 표기법 계산
- 수식의 괄호 검사 (연산자 우선순위 표현을 위한 괄호 검사)
- 함수의 콜스택, 깊이 우선 탐색(Depth-First Search, DFS) (재귀적 호출)
- 문자열 역순 출력

<br />

### Queue

<img src="https://user-images.githubusercontent.com/37680108/133214334-07118316-7a39-42c9-babe-e4b67691ccf7.png" style="float : left;">

- 데이터를 줄세운 형태의 선형 자료구조
- 선입선출 (First In First Out, FIFO)
- 놀이공원에 줄 서 있는 경우라고 쉽게 생각할 수 있다. 
- 삽입과 삭제가 큐의 양 끝에서 각각 수행된다.
  - enQueue : 삽입은 꼬리(rear)에서만 수행된다.
  - deQueue : 삭제는 머리(front)에서만 수행된다.

- 입출력을 반복하다 보면 front가 뒤로 가고 rear도 뒤로 가서 하나의 공간만 남게 된다.

  이러한 부분을 보완하기 위해 원형 큐의 개념이 등장했다.

  이외에도 우선순위 큐, Deque 등 다양한 형태의 큐가 있다.

  그 중 Deque은 삽입과 삭제가 양 방향에서 모두 일어난다.

- Java Colletion에서 Queue는 인터페이스이기 때문에 이를 구현하고 있는 Priority Queue 등을 사용할 수 있다

<br />

#### Queue의 활용

- 입력된 순서대로 작업을 수행해야 할 때 활용된다.
- 버퍼 (Buffer)
- 우선순위가 같은 작업 예약 (프린터의 인쇄 대기열, 고객 대기열)
- 프로세스 관리 (CPU 스케줄링)
- 너비 우선 탐색(Breadth-First Search, BFS) 구현
- 캐시(Cache) 구현



[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#Data-Structure)

<br />

## Tree
- 스택이나 큐와 같은 선형 구조가 아닌 비선형 자료구조
- 계층적 관계 (Hierarchical Relationship)을 표현하는 자료구조
- 부모 노드 밑에 여러 자식 노드가 연결되있고 자식노드 각각에도 다시 자식노드가 연결되는 비선형 재귀적 자료구조
- 자식노드 개수(차수)를 최대 2개로 제한하면 이진 트리가 되며 보통 왼쪽, 오른쪽 자식으로 나눔

### Tree 관련 용어
![image](https://user-images.githubusercontent.com/37680108/134206930-04aa2d33-15b2-4e8f-a5ca-6a6fcd1ac861.png)

|용어|설명|
|--|--|
|Node (노드) | 트리를 구성하고 있는 각각의 요소|
|Edge (간선) | 트리를 구성하기 위해 노드와 노드를 연결하는 선|
|Root Node (루트 노드) |트리 구조에서 최상위에 있는 노드|
|Leaf Node (Terminal Node, 단말 노드) |하위에 다른 노드가 연결되어 있지 않은 노드|
|Internal Node (내부 노드, 비단말 노드)|단말 노드를 제외한 모든 노드로 루트 노드도 포함|
|Sibling Node (Brother Node, 형제 노드) |동일한 부모를 가지는 노드|
|Path (경로)| 한 노드에서 다른 한 노드에 이르는 길 사이에 있는 노드들의 순서|
|Level (레벨) |루트 노드(level = 0)부터 노드까지 연결된 간선 수|
|Depth (깊이) | 루트 경로의 길이|
|Height (높이) | 가장 긴 루트 경로의 길이 (트리의 최대 레벨)|
|Degree (차수) |각 노드의 자식 노드의 개수|
|Degree of Tree (트리의 차수) | 트리에 있는 노드의 최대 차수|
|Subtree (서브트리) |큰 트리에 속하는 작은 트리|
|Forest (포레스트) | 서로 독립인 트리들의 모임|

<br>
[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#Data-Structure)
<br>

### Binary Tree (이진 트리)
> **degree가 최대 2인 트리** 
- 자식 노드로 최대 2개를 가짐 (공집합, 노드가 하나인 트리도 이진 트리에 포함)
- 구현시 `값 - 왼쪽 자식 노드 포인터 - 오른쪽 자식 노트 포인터`
- i번째 level에 나올 수 있는 노드의 최대 개수는 `2^(i-1)`
- tree의 depth가 k일 때, 최대 노드의 개수는 `2^k-1`
- 모든 트리는 이진 트리로 재구성할 수 있기 때문에 일반적으로 트리 구조는 이진 트리로 구현됨

#### 배열로 구성하는 경우
노드의 개수가 n개이고 root가 0이 아닌 1에서 시작할 때, i번째 노드에 대해서 부모, 자식 노드는 다음과 같은 index를 갖는다.
> parent(i) = i / 2  
> left_child(i) = 2 * i  
> right_child(i) = 2 * i + 1  

#### Binary Tree의 종류
- Full Binary Tree
- Complete Binary Tree
- Perfect Binary Tree
- Degenerate (or Pathological) Tree


<br>
[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#Data-Structure)
<br>

### Full Binary Tree (정 이진 트리)
> **모든 노드가 0개 혹은 2개의 자식 노드만을 갖는 이진 트리**

![Full Binary Tree](https://blog.martinwork.co.kr/images/datastructure/tree03.png)

- 자식이 1개인 노드는 없다.
- leaf 노드들을 제외한 모든 노드들이 2개의 children 을 가진다.
- L = leaf nodes 개수, I = internal nodes 개수일 때, L = I + 1
	- 즉, Full Binary Tree 에서 모든 leaf 노드의 개수는 internal node 의 개수 + 1 이다.


<br>
[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#Data-Structure)
<br>

### Complete Binary Tree (완전 이진 트리) `균형 트리`
> **위에서 아래로, 왼쪽에서 오른쪽으로 (좌측상단부터) 순서대로 차곡차곡 채워진 이진 트리**

![Complete Binary Tree](https://blog.martinwork.co.kr/images/datastructure/tree04.png)

- 마지막 level을 제외한 나머지 level에 node들이 가득 차있고, 마지막 level은 node가 가장 왼쪽 부터 채워지는 형태
  - 즉, 모든 노드의 오른쪽 자식이 있다면 왼쪽 자식이 있고, 왼쪽부터 차례로 채워나간 형태의 이진트리
- 왼쪽부터 빠짐없이 채워져 있으므로 **Array를 사용**하는 것이 일반적임
- 완전 이진 트리 구조를 그대로 사용하여 Binary Heap이라는 데이터 구조를 만들 수 있는데, 이것이 Heap
- 수학적 성질을 사용하기 용이하고, 배열을 이용해 구현하기도 용이하기 때문에 많이 쓰임


<br>
[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#Data-Structure)
<br>

### Perfect Binary Tree (포화 이진 트리) `균형 트리`
> **모든 internal node가 2개의 children을 가지고 있고, 모든 leaf 노드가 같은 level에 있는 이진트리**

![Perfect Binary Tree](https://blog.martinwork.co.kr/images/datastructure/tree05.png)

- complete이면서 full인 이진트리
- 모든 레벨이 꽉 찬 이진 트리
- 완벽한 이등변삼각형 모양으로 표현되는 이진 트리  
- Height 가 h인 Perfect Binary Tree는 `2h - 1`개의 노드를 가짐
- Proper Binary Tree라고도 함
- 보통 리프 노드 중 사용되지 않는 노드는 default값 (0, null, false 등) 이 들어감


<br>
[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#Data-Structure)
<br>

### Binary Search Tree (이진 탐색 트리)
- 노드의 왼쪽가지에는 노드의 값보다 작은값들만있고 오른쪽 가지에는 큰값들로만 이루어진 트리 => 이진탐색을 하기에 적합한 구성이 됨
- 최악의 경우 O(N)이 될수있어 이를보완한 AVL트리나 Red-Black트리를 사용하기도 함

<br>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#Data-Structure)

<br />
