# Algorithm

* [거품정렬, 선택정렬, 삽입정렬](#거품정렬-선택정렬-삽입정렬)

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
