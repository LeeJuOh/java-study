# 프로세스 vs 쓰레드

## **프로세스(Process)**

- 프로세스는 실행 중인 프로그램
    - 프로그램이란, 파일이 저장 장치에 저장되어 있지만 메모리에는 올라가 있지 않은 정적인 상태를 말한다.
    - 즉, 프로그램을 실행하는 순간 해당 파일은 컴퓨터 메모리에 올라가게 되고, 이 상태를 동적(動的)인 상태라고 하며 이 상태의 프로그램을 프로세스라고 한다.
- 따라서 프로세스는 os의 작업 단위
- 프로세스가 실행되기 위해서는 아래와 같은 여러 자원들이 필요하며 os로부터 할당받음
    - cpu time
    - memory
    - files
    - I/O devices
- 프로세스는 각각 독립된 메모리 영역(Code, Data, Stack, Heap의 구조)을 할당받는다.
    - text(code) section: 실행 코드
    - data section: 전역 변수
    - heap section: 동적 할당된 메모리
    - stack section: 임시 데이터 저장소 지역 변수, 함수 파라미터, 리턴 address 등
- 독립된 메모리 공간을 가지므로 프로세스가 다른 프로세스의 자원에 접근하려면 프로세스 간의 통신(IPC, inter-process communication)을 사용해야 한다.
- 5가지의 상태를 가짐
    - new: the process is being created.
    - running: Instructions are being executed
    - waiting : the process is waiting for some event to occur, 입출력 완료 및 수신
    - ready: the process is waiting to be assigned to a processor.
    - terminated: the process has finished execution.

  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9b2e26fe-8ff0-404a-a92a-d77fdbd9d85e/_2021-04-24__11.59.28.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9b2e26fe-8ff0-404a-a92a-d77fdbd9d85e/_2021-04-24__11.59.28.png)


## **프로세스 제어 블록(Process Control Block, PCB) or TCB**

- 운영체제가 시스템 내의 프로세스들을 관리하기 위해 프로세스마다 유지하는 정보들을 담는 커널 내 자료구조. 커널 주소공간의 data 영역에 존재한다.
- 프로세스를 생성과 동시에 PCB는 생성된다.
- 저장되는 정보
    - 프로세스 식별자(Process ID, PID) : 프로세스 식별번호
    - 프로세스 상태 : new, ready, running, waiting, terminated 등의 상태를 저장
    - 프로그램 카운터 : 프로세스가 다음에 실행할 명령어의 주소
    - CPU 레지스터
    - CPU 스케쥴링 정보 : 프로세스의 우선순위, 스케줄 큐에 대한 포인터 등
    - 메모리 관리 정보 : 페이지 테이블 또는 세그먼트 테이블 등과 같은 정보를 포함
    - 입출력 상태 정보 : 프로세스에 할당된 입출력 장치들과 열린 파일 목록
    - 어카운팅 정보 : 사용된 CPU 시간, 시간제한, 계정번호 등
- 문맥 교환(context switching)이 일어나면 이전 프로세스 정보를 pcb에 저장하고 새 프로세스의 정보를 pcb에서 읽어온다
    - 문맥교환: 하나의 프로세스가 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하도록 하기 위해, 이전의 프로세스의 상태(문맥)를 보관하고 새로운 프로세스의 상태를 적재하는 작업을 말한다
    - 시스템콜이나 인터럽트로 인해 CPU제어권이 운영체제로 넘어가는 경우에도 프로세스 문맥 중 일부를 PCB에 저장하기는 하지만 이 과정을 문맥 교환이라고 하지는 않는다. 단지 하나의 프로세스가 사용자 모드에서 커널 모드로 실행 모드만 바뀌는 것 뿐이기 때문이다.

## **스레드(Thread)**

- 프로세스 내에서 실행 흐름의 단위
- 스레드 ID, 프로그램 카운터, 레지스터 집합, 그리고 스택으로 구성
- 프로세스 내에서 실행되는 흐름의 단위
- 일반적으로 한 프로그램은 하나의 스레드를 가지고, 둘 이상의 스레드를 동시에 실행한다면 이를 멀티스레드(Multi-Thread)라 한다
- 스레드는 프로세스 내에서 stack만 따로 할당 받고, code, data, heap 영역은 공유한다

### **스택을 스레드마다 독립적으로 할당하는 이유**

- 스레드의 정의에 따라 독립적인 실행 흐름을 추가하기 위해 최소 조건으로 독립된 스택을 할당한다.
    - 스택은 함수 호출 시 전달되는 인자, 되돌아갈 주소값 및 함수 내에서 선언하는 변수 등을 저장하기 위해 사용되는 메모리 공간이다.
    - 스택 메모리 공간이 독립적이라는 것은 독립적인 함수 호출이 가능하다는 것이고 이는 독립적인 실행 흐름이 가능하게 한다.

### **PC Register 를 스레드마다 독립적으로 할당하는 이유**

- 명령어가 연속적으로 수행되지 못하기 때문에 어느 부분까지 수행했는지 기억하기 위해서 PC 레지스터를 독립적으로 할당한다.
    - PC 값은 스레드가 명령어의 어디까지 수행하였는지를 나타나게 된다
    - 스레드는 CPU 를 할당받았다가 스케줄러에 의해 다시 선점당한다.


## **멀티프로세스 vs 멀티스레드**

- `멀티 프로세스`는 한 프로세스에서의 오류가 타 프로세스에 영향을 미치지 않는다는 장점
- 하지만 `컨텍스트 스위칭의 비용`이 크고 어려운 `IPC`
- `멀티 스레드`는 적은 메모리 공간을 차지하고 문맥 전환이 빠르다는 장점
- 하지만 `동기화 문제`  와 하나의 스레드가 종료되면 `전체 스레드가 종료`될 수도 있는 것입니다.
- 따라서 시스템의 특성에 따라 멀티 프로세스/멀티 스레드를 잘 선택하는 것이 중요
    - 크롬 브라우저의 탭은 쓰레드일까 프로세스일까?

## 참고출처

- [https://velog.io/@adam2/2020-01-08-2301-작성됨-huk55f3cic](https://velog.io/@adam2/2020-01-08-2301-%EC%9E%91%EC%84%B1%EB%90%A8-huk55f3cic)
- [https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html](https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html)
- [https://velog.io/@yewon-july/Process-vs-Thread](https://velog.io/@yewon-july/Process-vs-Thread)
