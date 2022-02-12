# Queue

### 한문장 정리

- 선형 자료구조의 일종으로 First In First Out (FIFO)

### 큐

- 입력과 출력을 한 쪽 끝(front, rear)으로 제한
- FIFO (First In First Out, 선입선출) : 가장 먼저 들어온 것이 가장 먼저 나옴
- 큐의 가장 첫 원소를 front, 끝 원소를 rear라고 부름
- 데이터 넣음 : enQueue()
- 데이터 뺌 : deQueue()
- 비어있는 지 확인 : isEmpty()
- 꽉차있는 지 확인 : isFull()
- front : deQueue 할 위치 기억
- rear : enQueue 할 위치 기억21
- Insertion O(1)
- Deletion O(1)
- Search O(n)

### ***언제 사용?***

- 데이터가 입력된 시간 순서대로 처리해야 할 필요가 있는 상황에 이용한다.
- 너비 우선 탐색(BFS, Breadth-First Search) 구현
    - 처리해야 할 노드의 리스트를 저장하는 용도로 큐(Queue)를 사용한다.
    - 노드를 하나 처리할 때마다 해당 노드와 인접한 노드들을 큐에 다시 저장한다.
    - 노드를 접근한 순서대로 처리할 수 있다.
- 캐시(Cache) 구현
- 우선순위가 같은 작업 예약 (인쇄 대기열)
- 선입선출이 필요한 대기열 (티켓 카운터)
- 프린터의 출력 처리
- 프로세스 관리

### 구현

- 기본값

```java
private int size = 0; 
private int rear = -1; 
private int front = -1;

Queue(int size) { 
    this.size = size;
    this.queue = new Object[size];
}
```

- enQueue

```java
public void enQueue(Object o) {
    
    if(isFull()) {
        return;
    }
    
    queue[++rear] = o;
}
```

- deQueue

```java
public Object deQueue(Object o) {
    
    if(isEmpty()) { 
        return null;
    }
    
    Object o = queue[front];
    queue[front++] = null;
    return o;
}
```

- isEmpty

```java
public boolean isEmpty() {
    return front == rear;
}
```

- isFull

```java
public boolean isFull() {
    return (rear == queueSize-1);
}
```

### 연결리스트 큐

- enqueue
    - 데이터 추가는 끝 부분인 tail에 한다.
    - 기존의 tail는 보관하고, 새로운 tail 생성
    - 큐가 비었으면 head = tail를 통해 둘이 같은 노드를 가리키도록 한다.
    - 큐가 비어있지 않으면, 기존 tail의 next에 새로만든 tail를 설정해준다.

```java
public void enqueue(E item) {
    Node oldlast = tail; // 기존의 tail 임시 저장
    tail = new Node; // 새로운 tail 생성
    tail.item = item;
    tail.next = null;
    if(isEmpty()) head = tail; // 큐가 비어있으면 head와 tail 모두 같은 노드 가리킴
    else oldlast.next = tail; // 비어있지 않으면 기존 tail의 next = 새로운 tail로 설정
}
```

- dequeue
    - 데이터는 head로부터 꺼낸다. (가장 먼저 들어온 것부터 빼야하므로)
    - head의 데이터를 미리 저장해둔다.
    - 기존의 head를 그 다음 노드의 head로 설정한다.
    - 저장해둔 데이터를 return 해서 값을 빼온다.
