# Heap

## 한 문장 정리

- 힙은 특정한 규칙을 가지는 트리로, 최댓값과 최솟값을 찾는 연산을 빠르게 하기 위해 고안된 완전이진트리를 기본으로 한다.
- 완전 이진 트리의 일종으로 `우선순위 큐`가 바로 힙 자료구조를 사용한다.

## 우선순위 큐

- 우선순위의 개념을 큐에 도입한 자료 구조
- 시뮬레이션 시스템
- 네트워크 트래픽 제어
- 운영 체제에서의 작업 스케쥴링
- 수치 해석적인 계산
- 배열, 연결리스트, 힙 으로 구현이 가능하다. 이 중에서 힙(heap)으로 구현하는 것이 가장 효율적이다.
- 힙 → 삽입 : O(logn) , 삭제 : O(logn)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/36acecfc-ac5f-43ef-ac25-b97235859311/_2021-05-16__9.21.26.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/36acecfc-ac5f-43ef-ac25-b97235859311/_2021-05-16__9.21.26.png)

## 힙

- 완전 이진 트리의 일종으로 우선순위 큐를 위하여 만들어진 자료구조이다.
    1. 마지막 레벨을 제외한 모든 노드가 채워져있어야함
    2. 모든 노드들은 왼쪽부터 채워져있어야함
- 여러 개의 값들 중에서 최댓값이나 최솟값을 빠르게 찾아내도록 만들어진 자료구조이다.
    - 어떤 리스트에 값을 넣었다가 빼낼려고 할 때, 우선순위가 높은 것 부터 빼내려고 한다면 대개 정렬을 떠올리게 된다.
    - 쉽게 생각해서 숫자가 낮을 수록 우선순위가 높다고 가정할 때 매 번 새 원소가 들어올 때 마다 이미 리스트에 있던 원소들과 비교를 하고 정렬을 해야한다.
    - 문제는 이렇게 하면 비효율적이기 때문에 좀 더 효율이 좋게 만들기 위하여 다음과 같은 조건을 붙였다.
    - **`'부모 노드는 항상 자식 노드보다 우선순위가 높다.'`**
- 따라서 형제 간 우선순위는 고려되지 않기 때문에 힙은 일종의 `반정렬 상태(느슨한 정렬 상태)` 를 유지한다.
    - 큰 값이 상위 레벨에 있고 작은 값이 하위 레벨에 있다는 정도
    - 간단히 말하면 부모 노드의 키 값이 자식 노드의 키 값보다 항상 큰(작은) 이진 트리를 말한다.

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/15032dd7-2665-4102-9702-b45761403cff/Untitled.png)

- 힙 트리에서는 중복된 값을 허용한다. (이진 탐색 트리에서는 중복된 값을 허용하지 않는다.)

## 힙(heap)의 종류

- 최대 힙(max heap)
    - 부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같은 완전 이진 트리
    - key(부모 노드) >= key(자식 노드)
- 최소 힙(min heap)
    - 부모 노드의 키 값이 자식 노드의 키 값보다 작거나 같은 완전 이진 트리
    - key(부모 노드) <= key(자식 노드)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f53172d1-1452-49e3-b477-5f0283293b5e/_2021-05-16__9.24.02.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f53172d1-1452-49e3-b477-5f0283293b5e/_2021-05-16__9.24.02.png)

## 힙의 구현은 ??

- 가장 표준적으로 구현되는 방식은 **'배열'** 이다.
- 물론 연결리스트로도 구현이 가능하긴 하지만, 문제는 특정 노드의 '검색', '이동' 과정이 조금 더 번거롭기 때문이다.
- 배열의 경우는 특정 인덱스에 바로 접근할 수가 있기 때문에 좀 더 효율적이기도 하다.
- **[특징]**

    1. 구현의 용이함을 위해 시작 인덱스(root)는 1 부터 시작한다.

    2. 각 노드와 대응되는 배열의 인덱스는 '불변한다'

- **[성질]**

    1. 왼쪽 자식 노드 인덱스 = 부모 노드 인덱스 × 2

    2. 오른쪽 자식 노드 인덱스 = 부모 노드 인덱스 × 2 + 1

    3. 부모 노드 인덱스 = 자식 노드 인덱스 / 2


![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8bbf6eb9-5a7c-490c-97fc-b568d7474a3e/Untitled.png)

## 힙(heap)의 삽입

- 힙에 새로운 요소가 들어오면, 일단 새로운 노드를 힙의 마지막 노드에 이어서 삽입한다.
- 새로운 노드를 부모 노드들과 교환해서 힙의 성질을 만족시킨다.
    - 아래의 최대 힙(max heap)에 새로운 요소 8을 삽입해보자.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f5591f4f-695b-4101-a809-9dce57d6b6b5/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7aefbb21-52ff-4104-8bbc-7520baf9a99f/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d423efd-b5b1-4179-827f-97cba8c50257/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7183c059-2450-4afd-8ed1-7c1d332e6704/Untitled.png)

```java
/* 최대힙 삽입 */
void insert_max_heap(int x){
maxHeap[++heapSize] = x; // 힙 크기를 하나 증가하고 마지막 노드에 x를 넣는다.

for (int i=heapSize; i>1; i/=2) {
  // 마지막 노드가 자신의 부모 노드보다 크면 swap
  if (maxHeap[i/2] < maxHeap[i]) {
    swap(i/2, i);
  } else {
    break;
  }
}
}
https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html
```

## 힙(heap)의 삭제

- 최대 힙에서 최댓값은 루트 노드이므로 루트 노드가 삭제된다.
    - 최대 힙(max heap)에서 삭제 연산은 최댓값을 가진 요소를 삭제하는 것이다.
- 삭제된 루트 노드에는 힙의 마지막 노드를 가져온다.
- 힙을 재구성한다.
    - 아래의 최대 힙(max heap)에서 최댓값을 삭제해보자.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a100a575-ba29-490c-ba40-7e73f2e1bda2/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9343f7bc-e145-4e4a-8e52-1470ab849a79/Untitled.png)

```java
int delete_max_heap(){
if (heapSize == 0) // 배열이 빈 경우
  return 0;

int item = maxHeap[1]; // 루트 노드의 값을 저장한다.
maxHeap[1] = maxHeap[heapSize]; // 마지막 노드의 값을 루트 노드에 둔다.
maxHeap[heapSize--] = 0; // 힙 크기를 하나 줄이고 마지막 노드를 0으로 초기화한다.

for (int i=1; i*2<=heapSize;) {
  // 마지막 노드가 왼쪽 노드와 오른쪽 노드보다 크면 반복문을 나간다.
  if (maxHeap[i] > maxHeap[i*2] && maxHeap[i] > maxHeap[i*2+1]) {
    break;
  }
  // 왼쪽 노드가 더 큰 경우, 왼쪽 노드와 마지막 노드를 swap
  else if (maxHeap[i*2] > maxHeap[i*2+1]) {
    swap(i, i*2);
    i = i*2;
  }
  // 오른쪽 노드가 더 큰 경우, 오른쪽 노드와 마지막 노드를 swap
  else {
    swap(i, i*2+1);
    i = i*2+1;
  }
}
return item;
}
```

## 참고 출처

- [https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html](https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html), 자바, c
- [https://velog.io/@seanlion/pythonmaxheap](https://velog.io/@seanlion/pythonmaxheap), python
- [https://st-lab.tistory.com/205](https://st-lab.tistory.com/205)
