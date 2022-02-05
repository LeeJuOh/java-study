## 뮤텍스

- 뮤텍스는 리소스에 대한 액세스를 동기화하는데 사용되는 `locking 메커니즘`
- 하나의 태스크(쓰레드 or 프로세스일 수 있음)만 동일 시점에 뮤텍스를 가져올 수 있다.
    - 화장실이 한개이고 키도 1개
- 뮤텍스와 관련된 소유권이 있으며 `소유자`만 잠금(뮤텍스)을 해제할 수 있다.
    - 즉, 뮤텍스는 1개의 락만을 갖는 Locking 메커니즘

```java
wait (mutex); 
.... 
Critical Section
....
signal (mutex);
```

## 세마포어

- 세마포어는 `signaling` 메커니즘이다.
- 세마포어는 `락을 걸지 않은 쓰레드`도 Signal을 보내 `락을 해제`할 수 있다는 점에서 뮤텍스와 다르다.
    - 화장실이 여러개인 곳이라고 생각
- wait를 호출하면 세마포어의 카운트를 1줄이고, 세마포어의 카운트가 0보다 작거나 같아질 경우에 락이 실행된다.
- Counting Semaphores, Binary Semaphore 2종류
    - 바이너리 세마포어는 마치 뮤텍스처럼 동작

```java
P(S) {
     S--;
     if S < 0
         block();
 }

 V(S) {
     S++;
     if S <= 0
         wakeUp();
 }
```

## 일반 질문

---

1. 스레드가 둘 이상의 잠금(Mutex)을 획득할 수 있습니까?

- 예, 스레드에 둘 이상의 리소스가 필요할 수 있으므로 잠글 수 있습니다. 잠금을 사용할 수 없는 경우 스레드가 잠금 위치에서 대기(블록)합니다.
- critical section을 키를 하나로 둬서 관리하는건데

2. 뮤텍스를 두 번 이상 잠글 수 있습니까?

- 뮤텍스는 자물쇠입니다. 하나의 상태(잠금/잠금 해제)만 연결되어 있습니다.
- 그러나 재귀 뮤텍스는 두 번 이상(POSIX 호환 시스템) 잠글 수 있습니다.
- 여기서 카운트는 연결되어 있지만 하나의 상태(잠금/잠금 해제)만 유지합니다.
- 프로그래머는 뮤텍스가 잠긴 횟수만큼 잠금을 해제해야 합니다.

3. 비재귀 뮤텍스가 두 번 이상 잠기면 어떻게 됩니까?

- 교착 상태.
- 뮤텍스를 이미 잠근 스레드가 뮤텍스를 다시 잠그려고 하면 해당 뮤텍스의 대기 목록에 들어가 교착 상태가 발생합니다.
- 다른 어떤 쓰레드도 뮤텍스의 잠금을 해제할 수 없기 때문입니다.
- 운영 체제 구현자는 뮤텍스의 소유자를 식별하고 이미 동일한 스레드에 의해 잠긴 경우 반환하여 교착 상태를 방지할 수 있습니다.

4. 이진 세마포어와 뮤텍스가 같습니까?

- 아니요.
- 신호와 잠금 메커니즘에 설명되어 있으므로 별도로 취급하는 것이 좋습니다.
- 그러나 이진 세마포어에는 뮤텍스와 관련된 동일한 중요 문제(예: 우선 순위 반전)가 발생할 수 있습니다.
- 프로그래머는 카운트가 1인 세마포어를 만드는 것보다 뮤텍스를 선호할 수 있습니다.

5. 뮤텍스 및 critical section이란 무엇입니까?

- 일부 운영 체제는 API에서 동일한 단어 critical 섹션을 사용합니다.
- 일반적으로 뮤텍스는 관련된 보호 프로토콜로 인해 비용이 많이 드는 작업입니다.
- 마지막으로 mutex의 목적은 원자 접근입니다. 인터럽트를 비활성화하는 것과 같이 원자 접근을 달성하는 다른 방법도 있는데, 인터럽트는 훨씬 빠르지만 응답성은 망칩니다.
- 대체 API에서는 인터럽트를 사용하지 않도록 설정합니다.

6. 이벤트란 무엇입니까?

- mutex, semaphore, event, critical section 등의 의미는 동일합니다.
- 모두 동기화가 기본 사항입니다.

## 뮤텍스 베이커리 알고리즘

- 여러 프로세스/스레드에 대한 처리가 가능한 알고리즘. 가장 작은 수의 번호표를 가지고 있는 프로세스가 임계 구역에 진입한다.

```java
while(true) {
    
    isReady[i] = true; // 번호표 받을 준비
    number[i] = max(number[0~n-1]) + 1; // 현재 실행 중인 프로세스 중에 가장 큰 번호 배정 
    isReady[i] = false; // 번호표 수령 완료
    
    for(j = 0; j < n; j++) { // 모든 프로세스 번호표 비교
        while(isReady[j]); // 비교 프로세스가 번호표 받을 때까지 대기
        while(number[j] && number[j] < number[i] && j < i);
        
        // 프로세스 j가 번호표 가지고 있어야 함
        // 프로세스 j의 번호표 < 프로세스 i의 번호표
    }
}

// ------- 임계 구역 ---------

number[i] = 0; // 임계 구역 사용 종료
```

## 참고 출처

- [https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer Science/Operating System/Semaphore %26 Mutex.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Operating%20System/Semaphore%20%26%20Mutex.md)
- [https://www.geeksforgeeks.org/mutex-vs-semaphore/](https://www.geeksforgeeks.org/mutex-vs-semaphore/)
- [https://mangkyu.tistory.com/104](https://mangkyu.tistory.com/104)
