## **Red Black Tree**

- RBT(Red-Black Tree)는 BST 를 기반으로하는 트리 형식의 자료구조이다.
    - self-balancing binary search tree
- 이진 검색 트리를 기반으로 BST가 불균형해질 수 있는 문제를 해결하기 위한 각 노드에 1비트의 정보를 추가하여 red black 트리를 제공한다.
    - 이러한 색은 삽입 및 삭제 중에 트리가 균형을 유지하도록 하는 데 사용된다.
- 동일한 노드의 개수일 때, depth 를 최소화하여 시간 복잡도를 줄이는 것이 핵심 아이디어이다.
    - depth 가 최소가 되는 경우는 tree 가 complete binary tree(?) 인 경우이다.
    - Balanced binary tree인 것 같다.
- 삽입 및 삭제 후 트리 높이가 O(log n)로 유지되도록 하면 모든 연산에 대해 O(log n) 상한을 보장할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/389cc7ae-b465-417c-9a14-8e41d46f2bf8/Untitled.png)

## **Red-Black Tree 의 정의**

Red-Black Tree 는 다음의 성질들을 만족하는 BST 이다.

1. 각 노드는 `Red` or `Black`이라는 색깔을 갖는다.
2. Root node 의 색깔은 `Black`이다.
3. 각 leaf node 는 `Black`이다.
    - `널` 포인트이거나 명시적 노드(`NIL`)일 수 있다.
    - `NIL` 을 하나로 관리하면 메모리 절약 가능
4. `Red` 노드의  자식노드들의 색깔은 모두 `Black` 이다.
5. 노드(루트 포함)에서 리프 노드로 이어지는 모든 경로에는 동일한 수의 `Black` 노드가 포함된다.
    - black-height

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c57119d4-593c-4878-ba9e-dfc7a4e2fba8/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/100ede21-3049-4904-b668-398deaf59f94/Untitled.png)

### **Red-Black Tree 의 특징**

1. Binary Search Tree 이므로 BST 의 특징을 모두 갖는다.
2. Root node 부터 leaf node 까지의 모든 경로 중 최소 경로와 최대 경로의 크기 비율은 2 보다 크지 않다. 이러한 상태를 `balanced` 상태라고 한다.
3. 노드의 child 가 없을 경우 child 를 가리키는 포인터는 NIL 값을 저장한다. 이러한 NIL 들을 leaf node 로 간주한다.

## **삽입**

- 우선 BST 의 특성을 유지하면서 노드를 삽입을 한다.
- 그리고 삽입된 노드의 색깔을 **`RED` 로** 지정한다.
    - Red 로 지정하는 이유는 Black-Height 변경을 최소화하기 위함이다.

### insert 8

- 루트 노드 규치에 따라 흑색으로 삽입

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/255379c0-e026-4dae-8390-d667fd06bf3f/Untitled.png)

### insert 18, 5

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fbde481e-fce0-4e01-aebc-ba5cde7ef37f/Untitled.png)

### inser 15

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/951d8f1f-6cf6-4d4f-bdcf-65f999178cc9/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1bbb816d-1c89-46e1-b1bc-e5b2d2527002/Untitled.png)

- RED 노드가 연속적으로 나올 수 없다 규칙을 위반
- 연속된 RED가 나오게 된다면 이를 해결하기 위해 두 가지 해결방법을 사용하고 있다.
    - `Recoloring`
        - 삽입된 노드의 부모의 형제 색깔이 `RED`인 경우
    - `Restructuring`
        - 삽입된 노드의 부모의 형제 색깔이 `BLACK`인 경우, `NULL`인 경우
- 따라서 Recoloring 진행
    - `삽입된 노드의 부모`와 `부모 형제`노드를 `BLACK`
    - `부모의 부모`노드를 `RED`로 Coloring합니다.
    - 부모의 부모노드가 Root Node인 경우 `Root Node인 경우 Black인 규칙`에 의해 변경되지 않는다.
        - 부모의 부모노드가 Root node가 아닌 경우 `Double Red가 다시 발생` 할 수 있습니다.
