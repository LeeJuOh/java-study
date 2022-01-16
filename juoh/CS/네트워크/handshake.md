# TCP 3 way & 4way handshake

### 한문장 정리

- TCP 통신을 이용하여 데이터를 전송하기 위해  3-way handshaking 과정을 통해 연결을 설정하고, 4-way handshaking을 통해 연결을 해제한다.

## What is the SYN packet? ACK packet?

- SYN: synchronize sequence number
- ACK: acknowledgement
- TCP 헤더에는 Flag bit 존재, 6bit로 구성 Urg-Ack-Psh-Rst-Syn-Fin 순서

## 3 way handshake란?

- TCP/IP 프로토콜을 이용해서 통신을 하는 응용 프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.
- 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 데이터 전달이 시작하기 전에 한 쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다.
- 절차
    1. 클라이언트는 서버에 접속을 요청하는 `SYN`을 보낸다.
        - 클라이언트는 `SYN`을 보내고 서버가 `SYN/ACK`을 보내올 때까지 기다리는 **SYN-SENT 상태**가 된다.
    2. 서버는 클라이언트로부터 `SNY` 요청을 받고 클라이언트에서 요청을 수락한다는 `SYN/ACK`을 보낸다.
        - 요청을 보낸 서버는 클라이언트가 `ACK`을 보내올 때까지 기다리는 **SYN-RECEIVED 상태**가 된다.
        - 실제 데이터를 주고받을 때는 **상대방이 보낸 시퀀스 번호 + 상대방이 보낸 데이터의 byte**를 합쳐서 `ACK`을 만든다.
        - 하지만 핸드셰이킹 과정에서는 데이터를 주고받기 전이기 때문에 byte를 더할 수 없으니 순서 구분을 위해 **시퀀스 번호 + 1**을 한 `ACK`을 클라이언트에게 보낸다.
    3. 클라이언트가 서버에게 `ACK`을 보내게 되면 이후로는 연결이 이루어지고 데이터가 오가게 된다.
        - 서버가 클라이언트로부터 `ACK`을 받으면 연결이 성립된 시점부터 서버는 **ESTABLISHED 상태**이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4e81a170-6993-4a43-bdc5-22f1a0202f8c/Untitled.png)

### 4-way handshake란

- TCP의 연결을 해제(Connection Termination) 하는 과정
- 절차
    1. 클라이언트는 서버에게 연결을 종료한다는 `FIN` 플래그를 보낸다.
        - FIN을 보낸 클라이언트는 FIN-WAIT 상태가 된다.
    2. 서버는 FIN을 받고, 확인했다는 `ACK`를 클라이언트에게 보낸다.
        - ACK을 보낸 서버는 자신의 통신이 끝날 때까지 CLOSE-WAIT 상태를 유지한다.
    3. 연결을 종료할 준비가 끝나면 연결 해지를 위한 준비가 되었음을 알리기 위해 서버가 클라이언트에 `FIN`을 보낸다.
        - FIN을 보낸 서버는 LAST-ACK 상태가 된다.
    4. 클라이언트는 FIN을 받고, 확인했다는 `ACK`를 서버에게 보낸다.
        - 아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 TIME_WAIT을 통해 기다린다.

    5.  서버는 ACK를 받은 이후 소켓을 닫는다 (Closed)

    6.  TIME_WAIT 시간이 끝나면 클라이언트도 닫는다 (Closed)


![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/227bf8f9-4950-46f5-a27e-71f23d246e36/Untitled.png)

### 왜 3 WAY인지 2 way는 불가능?

- tcp connection은 양방향성 커넥션이라 2 WAY로는 불가능

### TCP의 연결 설정 과정(3단계)과 연결 종료 과정(4단계)이 단계가 차이나는 이유?

- A. Client가 데이터 전송을 마쳤다고 하더라도 Server는 아직 보낼 데이터가 남아있을 수 있기 때문에 일단 FIN에 대한 ACK만 보내고, 데이터를 모두 전송한 후에 자신도 FIN 메시지를 보내기 때문이다.
- 즉, 종료할 때는 더 예기치 못한 상황이 많아서 확인 과정이 복잡한 4 방향 핸드셰이킹을 사용한다.
    - [https://www.crocus.co.kr/1362](https://www.crocus.co.kr/1362)

### 만약 Server에서 FIN 플래그를 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 상황이 발생하면 어떻게 될까?

- A. 이러한 현상에 대비하여 Client는 Server로부터 FIN 플래그를 수신하더라도 일정시간(Default: 240sec)동안 세션을 남겨 놓고 잉여 패킷을 기다리는 과정을 거친다. (TIME_WAIT 과정)

### syn 패킷에 시퀀스 넘버는 랜덤값인 이유는?

- Connection을 맺을 때 사용하는 포트(Port)는 유한 범위 내에서 사용하고 시간이 지남에 따라 재사용된다.
- 과거에 사용된 포트 번호쌍을 사용할 때 난수가 아닌경우 이전 커넥션의 패킷으로 인식할 가능성이 있기 때문
