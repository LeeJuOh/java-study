# 인터럽트

### 한문장 정리

- 프로그램을 실행하는 도중에 예기치 않은 상황이 발생할 경우 현재 실행 중인 작업을 즉시 중단하고, 발생된 상황에 대한 우선 처리가 필요함을 CPU에게 알리는 것

### 인터럽트

- 프로그램을 실행하는 도중에 예기치 않은 상황이 발생할 경우 현재 실행 중인 작업을 즉시 중단하고, 발생된 상황에 대한 우선 처리가 필요함을 CPU에게 알리는 것
- 크게 외부, 내부, 소프트웨어 인터럽트로 나뉜다.
- 우선순위를 정의한 인터럽트 벡터 정보를 IDT(Interrupt Descriptor Table) 저장, 해당 인터럽트 처리 루틴(ISR)으로 분기

### 외부 인터럽트

- 전원 이상 인터럽트(Power fail interrupt) : 말그대로 정전, 파워 이상 등
- 기계 착오 인터럽트(Machine check interrupt) : CPU의 기능적인 오류
- 외부 신호 인터럽트(External interrupt)타이머에 의한 인터럽트 : Preemptive개념을 생각하면 된다. 자원이 할당된 시간이 다 끝난 경우키보드로 인터럽트 키를 누른 경우 : 대표적으로 Control + Alt + Delete외부장치로부터 인터럽트 요청이 있는 경우 : I/O 인터럽트 아님!! 다른 개념이다
- 입출력 인터럽트(I/O Interrupt)입출력장치가 데이터 전송을 요구하거나 전송이 끝나 다음 동작이 수행되어야 할 경우입출력 데이터에 이상이 있는 경우
- 우선순위 높음

### **내부 인터럽트**

- 잘못된 명령이나 잘못된 데이터를 사용할때 발생하며 Trap이라 부른다.
- 프로그래 검사 인터럽트
    - Program check interrupt
    - Division by zero
    - OverflowUnderflow기타 Exception
- 우선순위 보통

### **소프트웨어 인터럽트(SVC : SuperVisor Call)**

- 사용자가 프로그램을 실행시키거나 감시프로그램(Supervisor)을 호출하는 동작을 수행하는 경우
    - 사용자가 SVC명령을 써서 의도적으로 호출한 경우, 복잡한 입출력 처리를 해야하는 경우, 기억장치 할당 및 오퍼레이터와 대화를 해야하는경우 발생합니다.
    - 프로그램의 시스템 콜 요청 시 발생
    - 메모리 할당/해제, 자원 요청/반납 등
- 우선순위 낮음

### 인터럽트 처리 과정

- 인터럽트 발생 시 인터럽트 벡터 테이블 조회/분기, 처리루틴 수행, 복귀 3단계 절차로 동작
- 인터럽트 종류에 따라 우선순위 부여, 처리 순서 결정 위해 우선순위 처리 방법 필요
- 만약 인터럽트 기능이 없었다면, 컨트롤러는 특정한 어떤 일을 할 시기를 알기 위해 계속 체크를 해야 한다.
    - 이를 폴링(Polling) 이라고 한다
- 인터럽트 방식은 `하드웨어`로 지원을 받아야 하는 제약이 있지만, 폴링에 비해 신속하게 대응하는 것이 가능하다.
    - 따라서 `실시간 대응`이 필요할 때는 필수적인 기능이다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f983fb0-8064-4983-b768-58f1194e9365/_2021-05-16__10.27.51.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f983fb0-8064-4983-b768-58f1194e9365/_2021-05-16__10.27.51.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d641eaa1-1ca0-4976-b8ff-a2b9571d261c/_2021-05-16__10.28.58.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d641eaa1-1ca0-4976-b8ff-a2b9571d261c/_2021-05-16__10.28.58.png)

### 폴링과 인터럽트 차이

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6588fdbe-7007-4615-b7d4-732a03055b3f/_2021-05-16__10.31.57.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6588fdbe-7007-4615-b7d4-732a03055b3f/_2021-05-16__10.31.57.png)

### **DMA(Direct Memory Access) 직접 메모리 접근**

- 하지만 인터럽트 방식은 1 word를 처리할 때마다 CPU에 인터럽트 신호를 전송하는 방식이므로 여전히 CPU의 가용 시간을 잡아먹고 있다.
    - 그래서 이러한 문제를 해결하기 위해 CPU가 하는 일을 DMA controller가 대신하고 있다.
- DMA는 주변장치(하드디스크, 그래픽 카드 등)들이 메모리에 직접 접근하여 읽거나 쓸 수 있도록 하는 기능입니다.
- 중요한 건 **CPU의 개입 없이** I/O 장치와 기억장치 사이의 데이터를 전송하는 접근 방식이라는 거죠.
- PIO(Programmed Input/Output)은 CPU가 주변장치와 데이터를 주고받는 방식으로 효율이 떨어지는 방식입니다.
- 이를 극복하기 위해 DMA가 개발되었습니다.(IBM)
- 이렇게 좋은 DMA방식. 다이렉트로 전송하기 때문에 빠르지만 조심해야 한다.
    - **CPU - 메모리 간의 교류가 있을 때 외부 기기 - 메모리 간의 교류를 할 경우 메모리가 깨진다!**
- 그러므로 DMA를 하기 위해서는 DMA 관련 핸드셰이킹 프로토콜을 써서 CPU - 메모리 간의 연결이 없도록 만들어야 한다.

![https://blog.kakaocdn.net/dn/Lp6XL/btqzb9peeZz/Ck9TXsqtaeEXAv4OuXQxvk/img.jpg](https://blog.kakaocdn.net/dn/Lp6XL/btqzb9peeZz/Ck9TXsqtaeEXAv4OuXQxvk/img.jpg)

![https://blog.kakaocdn.net/dn/vs4dT/btqzbk54iUx/3tokKllxO5kgGbQpyLwAm0/img.jpg](https://blog.kakaocdn.net/dn/vs4dT/btqzbk54iUx/3tokKllxO5kgGbQpyLwAm0/img.jpg)

CPU 개입 없이라는 부분이 중요한 포인트입니다.

다른 관점으로 보면 CPU가 해야할 주변장치와의 데이터 전송을 DMA장치가 해주는 것이죠.

그만큼의 CPU 효율을 늘릴 수 있습니다.

고속의 I/O 장치의 경우 빈번한 인터럽트가 발생하는데 DMA를 사용함으로써

프로그램 수행 중 인터럽트의 발생 횟수를 최소화합니다.

### **동작 절차**

CPU 명령 - 버스 사용 요구(Bus Request) - 버스 사용 허가(Bus Grant) - 데이터 전송 - 인터럽트

### **동작모드**

1. 사이클 스틸링 : CPU가 DMA에 우선순위를 양보, 1 Cycle 정지, 빠른 입출력 가능, 한 번에 한 워드 전송

사이클 스틸링은 인터럽트와 많이 비교됨, 인터럽트는 1Cycle이 아니라 처리 기간 동안 정지

사이클 스틸링은 프로그램 상태 보존 필요 X, 반면에 인터럽트는 필요함

2. 버스트 모드 : DMA가 버스 사용권 획득 시 데이터 전송 완료까지 버스 사이클 독점, 블록 단위 데이터 전송
