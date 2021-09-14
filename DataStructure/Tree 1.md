# Tree 1

### 우지현

Tree

- 스택이나 큐와 같은 선형 구조가 아닌 **비선형 자료구조**
- 계층적 관계 (Hierarchical Relationship)을 표현하는 자료구조

Tree 관련 용어

<img src = "https://w.namu.la/s/606aecc8b8a27d42129f3e13c6db9a871a4566cd88c123689585256281efb5dde5b35f4e516572f0e5f0e419f0ae2be3aedf7a9c8dbb1756d1bf635a48da67ecd461dd60bccbd7b2b649dcf4ac2e98603e469257ee3261e6d2148154fbc6b0ce0f0e03ec1c9c6f832d6a50abcf0c05e2" width = "400" height="300" />

- Node (노드) : 트리를 구성하고 있는 각각의 요소
- Edge (간선) : 트리를 구성하기 위해 노드와 노드를 연결하는 선
- Root Node (루트 노드) : 트리 구조에서 최상위에 있는 노드
- Leaf Node (Terminal Node, 단말 노드) : 하위에 다른 노드가 연결되어 있지 않은 노드
- Internal Node (내부 노드, 비단말 노드) : 단말 노드를 제외한 모든 노드로 루트 노드도 포함
- Sibling Node (Brother Node, 형제 노드) : 동일한 부모를 가지는 노드
- Path (경로) : 한 노드에서 다른 한 노드에 이르는 길 사이에 있는 노드들의 순서
- Level (레벨) : 루트 노드(level = 0)부터 노드까지 연결된 간선 수
- Depth (깊이) : 루트 경로의 길이 
- Height (높이) : 가장 긴 루트 경로의 길이 (트리의 최대 레벨)
- Degree (차수) : 각 노드의 자식 노드의 개수
- Degree of Tree (트리의 차수) : 트리에 있는 노드의 최대 차수
- Subtree (서브트리) : 큰 트리에 속하는 작은 트리
- Forest (포레스트) : 서로 독립인 트리들의 모임

Binary Tree (이진 트리)

- 루트 노드를 중심으로 두 개의 서브 트리로 나뉘어진다.
- 또한 나뉘어진 두 서브 트리도 모두 이진 트리여야 한다.
- 공집합, 노드가 하나인 트리도 이진 트리에 포함된다.
- 배열로 구성된 Binary Tree는 노드의 개수가 n개이고 root가 0이 아닌 1에서 시작할 때, i번째 노드에 대해서 parent(i) = i / 2, left_child(i) = 2 * i, right_child(i) = 2 * i + 1의 index 값을 갖는다.
- Full Binary Tree, Complete Binary Tree, Perfect Binary Tree

Full Binary Tree (정 이진 트리)

![Full Binary Tree](https://blog.martinwork.co.kr/images/datastructure/tree03.png)

- 모든 노드가 0개 혹은 2개의 자식 노드만을 갖는 이진 트리

Complete Binary Tree (완전 이진 트리)

![Complete Binary Tree](https://blog.martinwork.co.kr/images/datastructure/tree04.png)

- 위에서 아래로, 왼쪽에서 오른쪽으로 순서대로 차곡차곡 채워진 이진 트리

Perfect Binary Tree (포화 이진 트리)

![Perfect Binary Tree](https://blog.martinwork.co.kr/images/datastructure/tree05.png)

- 모든 레벨이 꽉 찬 이진 트리


### 이수형

Tree

- 부모 노드 밑에 여러 자식 노드가 연결되있고 자식노드 각각에도 다시 자식노드가 연결되는 비선형 재귀적 자료구조이다.
- 이때 자식노드 개수(차수)를 최대 2개로 제한하면 이진 트리가 되며 보통 왼쪽,오른쪽 자식으로 나눈다.

이진트리

- 포화 이진트리
  - 모든 자식노드의 높이가 같고 리프노드가 아닌 노드는 모두 2개의 자식을 갖고있음
- 완전 이진트리
  - 모든 노드의 오른쪽 자식이 있다면 왼쪽 자식이 있는 이진트리 왼쪽부터 차레로 채워나간 형태
  - 완전 이진트리의 경우 왼쪽부터 빠짐없이 채워져 있으므로 배열을 이용해 N번째 원소의 왼쪽자식은 2N, 오른쪽자식은 2N+1원소로 꽉채울수있음
- 이진 탐색트리
  - 노드의 왼쪽가지에는 노드의 값보다 작은값들만있고 오른쪽 가지에는 큰값들로만 이루어진 트리 이진탐색을 하기에 적합한 구성이 됨
  - 최악의 경우 O(N)이 될수있어 이를보완한 AVL트리나 Red-Black트리를 사용하기도 함

### 김현수

#### Binary Tree 
- degree가 최대 2인 트리
- i번째 level에 나올 수 있는 노드의 최대 개수는 2^(i-1)
- tree의 depth가 k일 때, 최대 노드의 개수는 2^k-1

#### Binary Tree의 종류
- Full Binary Tree
- Perfect Binary Tree
- Complete Binary Tree
- Degenerate (or Pathological) Tree

#### Full Binary Tree(정 이진트리)
- leaf 노드들을 제외한 모든 노드들이 2개의 children 을 가지는 Binary Tree
- L = leaf nodes 개수, I = internal nodes 개수일 때, L = I + 1
	- 즉, Full Binary Tree 에서 모든 leaf 노드의 개수는 internal node 의 개수 + 1 이다.

#### Complete Binary Tree(완전 이진트리)
- 마지막 level을 제외한 나머지 level에 node들이 가득 차있고, 마지막 level은 node가 가장 왼쪽 부터 채워지는 형태
- Complete Binary Tree 구조를 그대로 사용하여 Binary Heap이라는 데이터 구조를 만들 수 있는데, 이것이 Heap이다.
- Complete Binary Tree(15개의 데이터가 저장된다면 index 0 ~ index 14 까지 채워진다) 구현에는 Array를 사용하는 것이 일반적이다.

#### Perfect Binary Tree(포화 이진트리)
- perfect binary tree는 complete이면서 full인 이진트리이다.
- 모든 internal node가 두개의 children을 가지고 있고, 모든 leaf 노드가 같은 level에 있는 이진트리
- Height 가 h인 Perfect Binary Tree는 2h - 1 개의 노드를 가진다.

#### 정리
Heap 은 Complete Binary Tree 형태를 가진다.<br>
Complete Binary Tree 구현에는 Array 를 사용하는 것이 일반적이다.<br>
Heap 은 Array 를 사용해서 구현하는 것이 편하다.<br>

### 이유진
#### Binary Tree (이진 트리)
- 자식 노드의 개수로 최대 2개를 갖는 트리
- 노드들의 구성 방식에 따라 (아래와 같이) 구분된다.

#### Full Binary Tree (정 이진 트리)
- 모든 노드가 0개 또는 2개의 child를 갖는 이진 트리

#### Perfect Binary Tree (포화 이진 트리) `균형 트리`
- 모든 내부 노드가 2개의 child를 갖는 이진 트리
- 모든 leaf 노드가 같은 level에 있다. 
- Proper Binary Tree라고도 한다.

#### Complete Binary Tree (완전 이진 트리) `균형 트리`
- 노드를 삽입할 때 왼쪽 노드부터 차례대로 채우는 이진 트리
