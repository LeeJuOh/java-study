# Stack

### 한문장 정리

- 선형 자료구조의 일종으로 Last In First Out (LIFO)

### **Stack**

- 선형 자료구조의 일종으로 `Last In First Out (LIFO)`. 즉, 나중에 들어간 원소가 먼저 나온다.
- 늦게 들어간 녀석들은 그 위에 쌓이게 되고 호출 시 가장 위에 있는 녀석이 호출되는 구조이다.
- 데이터 넣음 : push()
- 데이터 최상위 값 뺌 : pop()
- 비어있는 지 확인 : isEmpty()
- 꽉차있는 지 확인 : isFull()
- +SP
    - push와 pop할 때는 해당 위치를 알고 있어야 하므로 기억하고 있는 '스택 포인터(SP)'가 필요함
    - 스택 포인터는 다음 값이 들어갈 위치를 가리키고 있음 (처음 기본값은 -1)

    ```java
    private int sp = -1;
    ```

- Insertion O(1)
- Deletion O(1)
- Search O(n)

### ***언제 사용?***

- 함수의 콜스택, 문자열 역순 출력, 연산자 후위표기법

### 구현방법

- 배열

    ```java
    public void push(Object o) {
        if(isFull(o)) {
            return;
        }
        
        stack[++sp] = o;
    }
    ```

    ```java
    public Object pop() {
        
        if(isEmpty(sp)) {
            return null;
        }
        
        Object o = stack[sp--];
        return o;
        
    }
    ```

    ```java
    private boolean isEmpty(int cnt) {
        return sp == -1 ? true : false;
    }
    ```

    ```java
    private boolean isFull(int cnt) {
        return sp + 1 == MAX_SIZE ? true : false;
    }
    ```

    ```java
    // 동적 배열 스택, 최대 크기가 없는 스택 
    public void push(Object o) {
        
        if(isFull(sp)) {
            
            Object[] arr = new Object[MAX_SIZE * 2];
            System.arraycopy(stack, 0, arr, 0, MAX_SIZE);
            stack = arr;
            MAX_SIZE *= 2; // 2배로 증가
        }
        
        stack[sp++] = o;
    }
    ```


- 연결리스트

    ```java
    public class Node {
    
        public int data;
        public Node next;
    
        public Node() {
        }
    
        public Node(int data) {
            this.data = data;
            this.next = null;
        }
    }
    ```

    ```java
    public class Stack {
        private Node head;
        private Node top;
    
        public Stack() {
            head = top = null;
        }
    
        private Node createNode(int data) {
            return new Node(data);
        }
    
        private boolean isEmpty() {
            return top == null ? true : false;
        }
    
        public void push(int data) {
            if (isEmpty()) { // 스택이 비어있다면
                head = createNode(data);
                top = head;
            }
            else { //스택이 비어있지 않다면 마지막 위치를 찾아 새 노드를 연결시킨다.
                Node pointer = head;
    
                while (pointer.next != null)
                    pointer = pointer.next;
    
                pointer.next = createNode(data);
                top = pointer.next;
            }
        }
    
        public int pop() {
            int popData;
            if (!isEmpty()) { // 스택이 비어있지 않다면!! => 데이터가 있다면!!
                popData = top.data; // pop될 데이터를 미리 받아놓는다.
                Node pointer = head; // 현재 위치를 확인할 임시 노드 포인터
    
                if (head == top) // 데이터가 하나라면
                    head = top = null;
                else { // 데이터가 2개 이상이라면
                    while (pointer.next != top) // top을 가리키는 노드를 찾는다.
                        pointer = pointer.next;
    
                    pointer.next = null; // 마지막 노드의 연결을 끊는다.
                    top = pointer; // top을 이동시킨다.
                }
                return popData;
            }
            return -1; // -1은 데이터가 없다는 의미로 지정해둠.
    
        }
    
    }
    ```


### **Personal Recommendation**

- Stack 을 사용하여 미로찾기 구현하기
- Queue 를 사용하여 Heap 자료구조 구현하기
- Stack 두 개로 Queue 자료구조 구현하기
- Stack 으로 괄호 유효성 체크 코드 구현하기



## 참고 출처

- [https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer Science/Data Structure/Stack %26 Queue.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Data%20Structure/Stack%20%26%20Queue.md)
