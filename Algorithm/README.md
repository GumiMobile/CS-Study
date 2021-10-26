# Algorithm

* [거품정렬, 선택정렬, 삽입정렬](#거품정렬-선택정렬-삽입정렬)
* [병합정렬,퀵정렬](#병합정렬)
[뒤로](https://github.com/GumiMobile/CS-Study)

</br>

## 거품정렬, 선택정렬, 삽입정렬

### Bubble Sort (거품 정렬)
![Bubble Sort](https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/bubble-sort-001.gif)
- 서로 인접한 두 원소의 대소를 비교하고, 조건에 맞지 않다면 자리를 교환하며 정렬하는 알고리즘
- 첫번째 라운드가 끝나면 가장 큰 데이터가 맨뒤 N번째에 있게 되어서 2회차는 N-1번째까지만 정렬하고 3회차는 N-2번째까지 정렬하는 식으로 정렬해간다.
- 총 라운드는 (배열 크기-1) 번 진행되고, 각 라운드별 비교 횟수는 (배열 크기-라운드 횟수)이다.
- 시간 복잡도 : 최선, 평균, 최악 모두 `O(N^2)`
- 공간 복잡도 : 주어진 배열 안에서 교환(swap)을 통해 정렬이 수행되므로 `O(n)`
- 장점
  - 추가적인 메모리 소비가 적다.
  - 구현이 매우 쉽다.
  - 안정 정렬, 제자리 정렬이다.
- 단점
  - 다른 정렬 알고리즘에 비해 교환 과정이 많은 비효율적 알고리즘이다.
  
  <br>
  
### Selection Sort (선택 정렬)

![selection sort](https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/selection-sort-001.gif)

- 해당 순서에 원소를 넣을 위치는 정해진 상태에서 어떤 원소를 넣을지 선택하는 알고리즘
- 주어진 배열에서 최소값을 찾고 맨 앞에 위치한 값과 교체한다. 맨 처음 위치를 빼고 나머지 배열을 같은 방식으로 진행한다.
- 선택 정렬은 입력 배열 이외에 다른 추가 메모리를 요구하지 않는 방법이다.
- 시간 복잡도 : 최선, 평균, 최악 모두 `O(N^2)`
- 공간 복잡도 : 주어진 배열 안에서 교환(swap)을 통해 정렬이 수행되므로 `O(n)` 
- 장점
  - 알고리즘이 단순하다.
  - bubble sort보다는 빠르다.
  - 제자리 정렬이다.
- 단점
  - 비효율적이고, 불안정 정렬이다.

  <br>

### Insertion Sort (삽입 정렬)

![insertion sort](https://github.com/GimunLee/tech-refrigerator/raw/master/Algorithm/resources/insertion-sort-001.gif)

- 2번째 원소부터 시작하여 왼쪽의 원소들과 비교하여 삽입할 위치를 지정한 후, 원소를 뒤로 옮기고 지정된 자리에 자료를 삽입하여 정렬하는 알고리즘
- 시간 복잡도 : 최선의 경우 `O(N)`, 평균/최악의 경우 `O(N^2)`
- 공간 복잡도 : 주어진 배열 안에서 교환(swap)을 통해 정렬이 수행되므로 `O(n)`
- 장점
  - 알고리즘이 단순하다.
  - 대부분의 원소가 이미 정렬되어 있는 경우 매우 효율적일 수 있다.
  - In-place sort, Stable sort
  - Bubble Sort나 Selection Sort 같은 O(n^2) 알고리즘에 비교하여 상대적으로 빠르다.
- 단점
  - 평균과 최악의 시간 복잡도가 O(n^2)으로 비효율적이다.
  - Bubble Sort, Selection Sort와 마찬가지로 배열의 길이가 길어질수록 비효율적이다.
  - 데이터의 상태에 따라서 성능 편차가 매우 크다.

<br>

> **In-place Sort** (추가적인 메모리 공간을 많이 필요로 하지 않는 혹은 전혀 필요하지 않는 알고리즘) <br>
> **Stable Sort** (중복된 키를 순서대로 정렬하는 정렬 알고리즘) <br>
> **Unstable Sort** (동일한 key값을 정렬했을 때 순서가 바뀐다) <br>

  <br>

### Bubble, Selection, Insertion Sort 비교

| SORT   | BEST             | AVG | WORST |
|--------|------------------|-----|-------|
| Bubble | O(N<sup>2</sup>) | O(N<sup>2</sup>) | O(N<sup>2</sup>) |
| Selection | O(N<sup>2</sup>) | O(N<sup>2</sup>) | O(N<sup>2</sup>) |
| Insertion | O(N) | O(N<sup>2</sup>) | O(N<sup>2</sup>) |

3개의 정렬 방식 전부 구현이 단순하지만 O(N^2)인 정렬이므로 비효율적인 방법이라고 할 수 있다.

[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#algorithm)

## 병합정렬,퀵정렬

### 병합정렬

일반적인 방법으로 구현했을 때, 안정 정렬에 속하며, 분할 정복 알고리즘을 이용한 정렬 방법이다.

<img src="https://gmlwjd9405.github.io/images/algorithm-merge-sort/merge-sort-concepts.png" style width="700"/>

#### 과정

- 리스트의 길이가 0 또는 1이면 이미 정렬된 것으로 본다.
- 분할 (Divide) : 입력 배열을 같은 크기의 2개의 부분 배열로 분할한다.
- 정복 (Conquer) : 부분 배열을 정렬한다. 부분 배열의 크기가 충분히 작지 않으면 (1초과인 경우) 다시 분할 정복 방법을 적용한다.
- 결합 (Combine) : 정렬된 부분 배열들을 하나의 배열에 합병한다.

- 실제로 정렬이 이루어지는 시점은 2개의 부분 배열을 합병(merge)하는 단계이다.

<img src="http://www-scf.usc.edu/~zhan468/public/Notes/resources/CDDA3F11C6EFBC01577F5C29A9066772.gif" style width="600"/>

#### 장단점

- 장점
  - 안정적인 정렬방법으로 데이터의 분포에 관계없이 항상 O(n log n)을 만족한다.
  - 레코드를 linked list로 구성하면, 링크 인덱스만 변경되므로 데이터의 이동은 작아진다. 즉, 제자리 정렬로 구현할 수 있다.
- 단점
  - 레코드를 배열로 구성하면 임시 배열이 필요하다. 즉 제자리 정렬이 아니다.
  - 레코드들의 크기가 큰 경우, 이동횟수가 많아 시간 낭비가 발생한다.

- 퀵 정렬과의 차이점

  - 퀵 정렬 : 우선 피벗을 통해 정렬 (partition) -> 영역을 분할한다 (quickSort)
  - 병합 정렬 : 영역을 분할할 수 있을 만큼 나눈다 (mergeSort) -> 정렬 (merge)

- 합병 정렬은 순차적인 비교로 정렬을 진행하므로, LinkedList의 정렬이 필요할 때 사용하면 효율적이다.

  - LinkedList를 퀵 정렬로 정렬하면?

    > 퀵 정렬은 순차 접근(sequential access)이 아닌 임의 접근(random aceess)이기 때문에 성능이 좋지 않다.

  - LinkedList는 삽입, 삭제 연산에서 유용하지만 접근 연산에서는 비효율적이기 때문에 임의로 접근하는 퀵 정렬을 활용하면 오버헤드 발생이 증가한다.

    > 배열은 인덱스를 이용해서 접근이 가능하지만 LinkedList는 head부터 탐색해야 한다.
    >
    > 배열 `O(1)` vs LinkedList `O(n)` 


### 퀵정렬

- **불안정 정렬**에 속함
- **분할 정복** 알고리즘, 평균적으로 **매우 빠른 속도를 자랑**한다!
- 병합 정렬과 달리, 배열을 비균등하게 분할함

<img src="http://www-scf.usc.edu/~zhan468/public/Notes/resources/C411339B79F92499DCB7B5F304C826F4.gif" style width="600"/>

#### 과정

1. 피봇을 선택한다. (첫 요소, 중간 요소, 끝 요소, 랜덤으로 선택할 수 있다.)

2. 오른쪽(j)에서 왼쪽으로 가면서 피봇보다 작은 수를 찾는다.

3. 왼쪽(i)에서 오른쪽으로 가면서 피봇보다 큰 수를 찾는다.

4. i, j의 요소를 교환한다.

5. 위 2, 3, 4 과정을 반복한다.
6. 2, 3번을 더 이상 진행할 수 없다면, 피봇과 교환한다.
7. 그 결과는 피벗을 중심으로 왼쪽은 피봇보다 작은 수, 오른쪽은 피봇보다 큰 수들이 존재한다.

#### 장단점

- 장점
   - 재귀 방식을 이용하여 코드가 간결하다.
   - 속도가 빠르다. 시간복잡도 `O(NlogN)` (대체로 실제로는 더 빠르다. )
      - `합병단계(=순환 호출의 깊이=N)` * `각 단계의 비교 연산(=logN)` = NlogN
      - 퀵 정렬은 불필요한 데이터 이동을 줄이고, 한 번 결정된 pivot은 추후 연산에서 제외되므로 빠르다!
   - 추가 메모리 공간을 필요로 하지 않는다.
   - 정렬된 리스트에 대해서는 불균형 분할에 의해 오히려 수행시간이 더 많이 걸린다.
      - 리스트가 계속 불균형하게 나누어지는 경우 비효율적이다.
      - 따라서, pivot을 선택할 때 **리스트를 균등하게 분할할 수 있는 데이터를 선택**하는 것이 유리하다. (ex. 중간 값)
- 단점
   - `불안정 정렬 (Unstable Sort)`
   - 정렬된 배열에 대해서는 퀵 정렬의 불균형 분할에 의해 오히려 수행시간이 더 많이 걸린다.( O(N^2)) 

| 이름     | Best    | Avg     | Worst   | 공간        | 안정 정렬? |
| -------- | ------- | ------- | ------- | ----------- | ---------- |
| 병합정렬 | n log n | n log n | n log n | n           | o          |
| 퀵정렬   | n log n | n log n | n^2     | log n, O(n) | x          |


[뒤로](https://github.com/GumiMobile/CS-Study) / [위로](#algorithm)
