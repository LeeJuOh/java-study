# 운영체제란

## 운영체제란

- 일반적으로 `하드웨어를 관리하고, 응용 프로그램과 하드웨어 사이에서 인터페이스 역할을 하며 시스템의 동작을 제어하는 시스템 소프트웨어`
- 컴퓨터와 하드웨어 바로 위에 설치되어 사용자 및 다른 모든 소프트웨어와 하드웨어를 연결하는 소프트 계층
- 운영체제는 컴퓨터의 `성능`을 높이고(performance), 사용자에게 `편의성 제공`(Convenience)을 목적으로 하는 컴퓨터 하드웨어 관리하는 프로그램이다.
- the one program running at all times on the computer
- usually called the kernel.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/833c35cf-15ff-4156-b423-20521d17f80f/_2021-04-24__12.18.04.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/833c35cf-15ff-4156-b423-20521d17f80f/_2021-04-24__12.18.04.png)

- 운영체제는 크게 커널(kernel)과 명령어 해석기(Command interpreter, shell)로 나뉜다.
- 커널은 말그대로 운영체제의 핵심으로 **운영체제가 수행하는 모든 것**이 저장되어있다. 명령어 해석기는 사용자가 **커널(운영체제)에 요청하는 명령어를 해석하여 커널에 요청하고 그 결과를 출력**한다.
- 사용자는 GUI(Graphical User Interface)나 CLI(Command Line Interface) 같은 방식으로 운영체제에 명령을 요청할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/72edb58c-2c75-4d0a-a1e9-59a3f6fee1e1/Untitled.png)

## 부팅

- Processor는 일반적으로 CPU를 말한다. main memory를 보면 ROM과 RAM으로 나누어져 있다.
    - ROM: **비휘발성** 으로 메모리에서 극히 일부를 차지한다.(수 KB)
    - RAM: **휘발성** 으로 메모리의 대부분을 차지하며 실제 프로그램이 할당되는 곳이다.(수 MB ~ 수 GB)
- ROM은 하드디스크와 같이 비휘발성으로 전원이 꺼져도 그 안의 내용이 계속 유지된다. RAM은 휘발성이므로 전원이 꺼지면 메모리안의 모든 내용이 지워진다.
- 부팅 과정
    - ROM안에는 POST(Power-On Self-Test), 부트 로더(boot loader)가 저장되어 있다.
    - POST는 전원이 켜지면 가장 처음에 실행되는 프로그램으로 현재 컴퓨터의 상태를 검사한다.
    - POST 작업이 끝나면 부트 로더가 실행된다.
    - 부트 로더는 하드디스크에 저장되어 있는 운영체제를 찾아서 메인 메모리(RAM)에 가지고 온다.
    - 이러한 부트 로더의 과정을 **부팅**이라고 한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4d04f69c-7d59-4fa7-8f09-da7172bbc4cf/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9a06aae2-882a-4695-9cee-838e765d365e/Untitled.png)

## 운영체제의 역할

- User interface
- Program execution
- I/O operation
- File-system manipulation
- Communications
- Error detection
- Resource allocation
- Logging
- Protection and security

## **멀티 ~ (Multi ~ )**

---

### 멀티 프로세싱(Multi-processing)

- 멀티 프로세싱은 `다수의 프로세서`가 서로 협력적으로 작업을 병렬로 처리하는 것을 의미한다.
- 위에서 설명한 '프로세스'가 아닌 '프로세서'를 말하는 것
- 프로세서는 대략적으로 CPU라고 생각

[https://t1.daumcdn.net/cfile/tistory/99602E3359F1B08B23](https://t1.daumcdn.net/cfile/tistory/99602E3359F1B08B23)

- 각각의 프로세서가 하나의 작업만을 처리하는 것이 아니라 다수의 작업을 처리하며,
- 하나의 작업은 하나의 프로세서에 의해 처리되는 것이 아니라 다수의 프로세서에 의해 처리됩니다.
- 장점
    - 프로세서를 여러 개 사용하여 여러 개의 작업을 동시에 수행함으로써 작업 속도를 높일 수 있다.
    - 프로세서 중 일부에 문제가 발생하더라도 다른 프로세서를 이용해 처리할 수 있으므로 신뢰성이 높다.

### 멀티 프로그래밍(Multi-programming)

- 특정 프로세스 A에 대해서 프로세서가 작업을 처리할때 낭비되는 시간동안 다른 프로세스를 처리하도록 하는 것
    - 예를 들어 A라는 프로세스를 처리중에 있을때 입출력 이벤트가 발생했는데 프로세서가 입출력 이벤트에 대한 응답을 위해 무작정 대기하고 있다면 프로세서의 자원을 낭비하는 결과를 초래
    - 프로세서, CPU는 한번에 하나의 프로세스만 처리하도록 되어있기 때문에 A 프로세스에 대한 입출력 이벤트에 대한 응답을 대기하는 동안 아무일도 하지 않기 때문
- 멀티 프로그래밍은 이렇게 낭비되는 시간동안 프로세서가 다른 프로세스를 수행할 수 있도록 하는 것 입니다.
- 즉, 주기억장치에 적재된 여러 개의 프로그램들을 CPU가 항상 수행하도록 하여 `CPU 이용률`을 증진시키기 때문이다.
- 다중 프로그래밍 운영체제에서 여러 개의 작업들이 수행할 준비를 갖추고 있다면 이 작업들 중에 하나를 선택하기 위해서는 결정이 필요한데, 이것이 `CPU 스케줄링`이다.

### **멀티 테스킹(Multi-tasking)**

- 멀티 프로그래밍의 논리적 확장
    - 마찬가지로 cpu 스케줄링 필요
- 멀티 테스킹이란 다수의 Task(프로세스보다 보다 확장된 개념이라고 생각하시면 됩니다.)를 운영체제의 스케줄링에 의해 번갈아 가면서 수행하는 것 입니다.
- 프로세서가 각각의 Task를 조금씩 자주 번갈아가면서 처리하기 때문에 사용자는 마치 동시에 여러 Task가 수행되는 것처럼 보게 됩니다.
- 멀티프로그래밍과의 차이
    - 멀티프로그래밍은 프로세서의 자원이 낭비되는 것을 최소화하기 위한 것
    - 멀티테스킹은 `일정하게 정해진 시간`동안 번갈아가면서 각각의 Task를 처리하는 것


### **멀티 스레딩(Multi-threading)**

- 하나의 프로세스를 다수의 실행 단위(쓰레드)로 구분하여 자원을 공유하고 자원의 생성과 관리의 중복성을 최소화하여 수행 능력을 향상 시키는 것
- 또한 이런 다수의 스레드는 하나의 데이터 자원을 공유하기 때문에 메모리에 대한 효율성을 가질 수 있습니다.
- 멀티 프로세싱과의 차이
    - 멀티 스레딩은 하나의 프로그램 안에서 병렬 처리의 이점을 보는 것이며
    - 멀티 프로세싱은 여러 개의 프로그램들을 병렬로 처리할 수 있는 것 입니다.
- 멀티 스레딩은 스레드 수준뿐 아니라 명령어 수준의 병렬 처리에까지 신경을 쓰면서 하나의 코어에 대한 이용성을 증가하는 것에 초점을 두고 있습니다.

## 참고출처

- [https://velog.io/@codemcd/운영체제OS-1.-운영체제란](https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-1.-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C%EB%9E%80)
- [https://doorbw.tistory.com/26](https://doorbw.tistory.com/26)
