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
* [Heap](#heap)
  * [Binary Heap](#binary-heap)
  * [종류](#종류)
  * [구현](#구현)
  * [삽입](#삽입)
  * [삭제](#삭제)
* [Hash Table](#Hash-Table)
  * [정의](#정의)
  * [해시 함수](#해시-함수)
  * [해시 충돌](#해시충돌)
  * [충돌 해결 방법](#충돌-해결-방법)
  * [Resizing](#Resizing)
  * [Hash Table vs Hash Map vs Hash Set](#Hash-Table-vs-Hash-Map-vs-Hash-Set)

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



[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

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



[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

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

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

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

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

<br>

### Full Binary Tree (정 이진 트리)
> **모든 노드가 0개 혹은 2개의 자식 노드만을 갖는 이진 트리**

![Full Binary Tree](https://blog.martinwork.co.kr/images/datastructure/tree03.png)

- 자식이 1개인 노드는 없다.
- leaf 노드들을 제외한 모든 노드들이 2개의 children 을 가진다.
- L = leaf nodes 개수, I = internal nodes 개수일 때, L = I + 1
	- 즉, Full Binary Tree 에서 모든 leaf 노드의 개수는 internal node 의 개수 + 1 이다.


<br>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

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

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

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

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

<br>

### Binary Search Tree (이진 탐색 트리)
- 노드의 왼쪽가지에는 노드의 값보다 작은값들만있고 오른쪽 가지에는 큰값들로만 이루어진 트리 => 이진탐색을 하기에 적합한 구성이 됨
- 최악의 경우 O(N)이 될수있어 이를보완한 AVL트리나 Red-Black트리를 사용하기도 함

<br>

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

<br />

## Heap

- 정식명은 Heap tree, 여러 개의 값 중에서 가장 크거나 작은 값을 빠르게 찾기 위해 만든 트리이다.

- 큰 값이 상위 레벨에 있고 작은 값이 하위 레벨에 있다는 정도의 정렬 상태로 반 정렬 상태(느슨한 정렬 상태)라고 한다. 간단히 말하면 부모 노드의 키 값이 자식 노드의 키 값보다 항상 큰(작은) 트리를 말한다.

- 가장 큰(작은) 값을 알아내면 되기 때문에 전체 데이터를 정렬할 필요는 없다.

- 힙 트리에서는 이진 탐색 트리와 달리 중복된 값을 허용한다.

- 최댓값, 최솟값은 O(1) 만에 찾을수 있지만 삽입 삭제 경우에는 O(log N) 의 시간이 필요하다. 따라서 우선순위 큐 구현에 적합하다.

### Binary Heap

- 이진 트리 형태의 힙이다. 힙 중에서 가장 널리 쓰이는 형태이므로 보통 힙이라 함은 이진 힙을 뜻한다.
- 완전 이진 트리이다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

### 종류

![image](https://user-images.githubusercontent.com/76988389/135033590-2b23b17b-0da9-4011-aba8-abc23da7fe0c.png)

- 최소 힙 (Min Heap)

  부모 노드의 키 값이 자식 노드의 키 값보다 작거나 같은 완전 이진 트리

- 최대 힙 (Max Heap)

  부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같은 완전 이진 트리

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

### 구현

- 표준적인 자료구조는 배열이며, 0번째 인덱스는 건너뛰고 1번째 인덱스의 루트노드부터 시작된다.
  노드의 고유번호 값과 배열의 인덱스를 일치시켜 혼동을 피하기 위함이다.

> 💡 부모 노드와 자식 노드의 관계
>
> 왼쪽 자식 index = (부모 index) * 2
>
> 오른쪽 자식 index = (부모 index) * 2 + 1
>
> 부모 index = (자식 index) / 2

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

### 삽입

> 1. 가장 끝의 자리에 노드 삽입
> 2. 그 노드와 부모 노드를 비교하여 규칙(최대/최소)에 맞으면 보존, 아니면 교환
> 3. 규칙에 맞을 때까지 2번 과정 반복

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

### 삭제

> 1. 루트 노드(최상위 노드)를 제거
> 2. 그 자리에 가장 마지막 노드 삽입
> 3. 올라간 노트와 자식 노드들을 비교
> 4. 1. (최대 힙) 부모보다 더 큰 자식들 중 큰 값과 교환, 없으면 종료
>    2. (최소 힙) 부모보다 더 작은 자식들 중 작은 값과 교환, 없으면 종료
> 5. 4번을 반복

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)
## Hash Table

### 정의

해시 테이블은 <Key, Value>로 데이터를 저장하는 자료 구조 중 하나이다. Key값에 해시 함수를 적용해 index를 생성하고, index를 활용하여 값을 저장, 검색 할 수 있다. Key값을 해싱하여 검색, 저장하므로 평균 시간 복잡도는 O(1)이다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

### 해시 함수

해시 함수는 임의의 길이의 데이터를 고정된 길이의 데이터로 매핑하는 함수이다. 해시 함수의 성능은 입력 영역에서 해시 충돌 확률로 결정되는데, 해시 충돌 확률이 높을수록 서로 다른 데이터를 구별하기 어려워지고 검색하는 비용이 증가한다. 해시 테이블에서는 크게 4가지 해시 함수가 사용된다.

- **Division Method**

  모듈러 연산을 이용하는 방법으로 입력값을 테이블의 크기로 나누어 계산한다.

  주소 = 입력값 % 테이블 크기

- **Digit Folding Method**

  각 Key의 문자열을 ASCII 코드로 바꾼 뒤, 그 값의 합을 주소로 사용하는 방법이다.

- **Multiplication Method**

  숫자로 된 Key와 0과 1 사이의 실수, 보통 2의 제곱수를 사용하여 해시값을 계산한다.

  Hash(Key) = (<u>Key</u> * <u>R(0..1)</u> % 1) * <u>2^n</u>

- **Universal Hashing**

  다수의 해시 함수를 만들어 집합 H에 넣어두고, **무작위**로 해시함수를 선택해 해시값을 만든다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

### 해시충돌

해시 충돌은 해시함수가 서로 다른 두 개의 입력값에 대해 동일한 출력값을 내는 상황을 말한다. 따라서 서로 다른 Key값이 동일한 index로 매핑 될 경우, 해시 충돌이 발생하여 해시 테이블의 성능을 저하시킨다.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/5/58/Hash_table_4_1_1_0_0_1_0_LL.svg/240px-Hash_table_4_1_1_0_0_1_0_LL.svg.png)

위 그림에서 John Smith와 Sandra Dee라는 키의 해시가 서로 충돌을 일으킨다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

### 충돌 해결 방법

- 분리 연결법

  동일한 버킷의 데이터에 대해 추가 메모리와 자료구조를 사용하여 다음 데이터의 주소를 저장하는 것이다. 보통 linked list와 tree를 사용하여 구현한다.

  - Linked List

    각각의 버킷을 linked list로 만들어 충돌이 발생하면 해당 버킷의 list에 추가하는 방식

  - Tree

    linked list 대신 tree를 사용하며, 데이터 수가 많을 때 사용한다.

  <img src="https://media.vlpt.us/post-images/cyranocoding/329e7e60-b226-11e9-a4ce-730fc6b3757a/16eBeaqTti8MxWPsw4xBgw.png" style width = "400" height/>

  

- 개방 주소법

  기존의 해시 테이블의 비어있는 공간을 활용하는 방법이다. 비어있는 공간을 찾는 방법은 크게 3가지 방법이 있다.

  - Linear Probing

    충돌이 난 지점부터 순차적으로 탐색하며 비어있는 버킷을 찾는다.

  - Quadratic Probing

    2차 함수를 사용하여 비어있는 버킷을 찾는다.

  - Double Hashing Probing

    충돌이 발생하면 2차 해시 함수를 사용하여 새로운 주소를 할당한다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

### Resizing

해시 테이블은 Key, Value쌍이 늘어날수록 해시 충돌 확률이 올라가며 성능이 떨어진다. 기준 임계점은 빈공간 대비 75%가 사용될 때이고, 해시 버킷의 수를 두배로 늘린다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)

### Hash Table vs Hash Map vs Hash Set

3개의 자료구조는 모두 해시함수를 사용한다. 그러나 다음과 같은 차이가 있다.

|            | Hash Table | Hash Map | Hash Set |
| ---------- | ---------- | -------- | -------- |
| 동기화     | o          | x        | x        |
| 값의 중복  | o          | o        | x        |
| 인터페이스 | Map        | Map      | Set      |

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#data-structure)
