# Heap (Binary Heap)
## 이유진
### 힙(Heap)
완전 이진 트리의 일종으로, 여러 값 중 최대값과 최소값을 빠르게 찾아내도록 만들어진 자료구조. 우선순위 큐에서 사용된다.
(중복된 값 허용)

### 종류
- 최대 힙 (Max Heap)  
부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같은 완전 이진 트리
- 최소 힙 (Min Heap)  
부모 노드의 키 값이 자식 노드의 키 값보다 작거나 같은 완전 이진 트리

### 구현 by 배열
쉬운 구현을 위해 인덱스 0 은 사용하지 않는다.

#### 부모노드와 자식노드 index
> 왼쪽 자식 index = (부모 index) * 2
> 오른쪽 자식 index = (부모 index) * 2 + 1
> 부모 index = (자식 index) / 2

#### 삽입
1. 새로운 요소를 힙의 마지막 노드에 삽입한다. (`heapSize+1`)
2. (maxHeap기준) 마지막 노드가 부모 노드보다 크면 swap ... (부모 노드가 더 클 때까지 반복)  

#### 삭제(최대값 찾기)
1. 루트 노드가 삭제된다. : 시간복잡도 O(1)
2. 힙의 마지막 노드를 루트 노드로 가져온다.
3. 힙을 재구성한다. <= `Heapify` : 시간복잡도 O(logN)
