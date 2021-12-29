# OSI 7계층

### 한문장 정리
- OSI (Open System Interconnection) 7계층은 국제표준화기구에서 개발한 네트워크 표준 모델
- 컴퓨터 네트워크 프로토콜 디자인과 통신을 계층으로 나누어 설명한 개방형 시스템 상호 연결 모델이다.
- 인터넷 프로토콜 후에 등장

## 7계층으로 나누는 이유

- 각 계층이 통신이 일어나는 과정을 단계별로 알 수 있고, 독립적으로 되어 있어 특정한 곳에 이상이 생기면 그 단계만 수정할 수 있기 때문

## Encapsulation & Decapsulation

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/03c4848a-b403-4cf9-81a2-02865de4f86d/Untitled.png)

- Encapsulation
    - 데이터를 전송할 때 각각의 레어마다 인식할 수 있는 헤더를 붙이는 과정
    - 2계층(Data layer, 데이터링크계층)에서는 오류제어를 위해 데이터의 뒷부분에도 일부 데이터가 추가됨
- Decapsulation
    - 수진된 데이터가 각각의 레이어를 따라 올라가면서 헤더가 벗겨지는 과정

## OSI 7 Layer별 Protocol과 기능

- 통신이 일어나는 과정을 단계별로 알 수 있고, 특정한 곳에 이상이 생기면 그 단계만 수정할 수 있기 때문이다
- 1계층 ~ 4계층 : 하위 계층
- 5계층 ~ 7계층 : 상위 계층
- 하위계층으로 갈수록 하드웨어에 가까워지고, 상위 계층으로 갈수록 소프트웨어에 가깝다
- 각 계층은 하위 계층의 기능만을 이용하고, 상위 계층에게 기능을 제공한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a7e69928-8683-4650-aa88-371d5651772c/simple-osi-model-7-layers.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a7e69928-8683-4650-aa88-371d5651772c/simple-osi-model-7-layers.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4df76b5b-2b05-46c8-9f1a-ebbb85ff1065/Untitled.png)

## OSI 계층 설명

### 1계층: 물리 계층

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f6b7057a-bb55-48ca-84bd-2656ca3ecef1/Untitled.png)

- 물리적 계층은 네트워크 노드 간의 물리적 케이블 또는 무선 연결을 담당한다.
- 비트(Bit)단위의 PDU
- 커넥터, 장치를 연결하는 전기 케이블 또는 무선 기술을 정의하고 비트 전송률 제어를 처리하면서 단순히 0과 1의 연속인 원시 데이터의 전송을 담당합니다.
- 1계층 장비 : 케이블, 리피터, 허브

### 2계층:  데이터링크 계층

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f2cfd766-08a4-4895-bc8c-bb64bfffa8cf/Untitled.png)

- 데이터 링크 계층은 네트워크에서 물리적으로 연결된 두 노드 간의 연결을 설정하고 종료한다.
- 패킷을 프레임으로 분할하고 소스에서 대상으로 보낸다. 이 계층은 네트워크 프로토콜을 식별하고 오류 검사를 수행하고 프레임을 동기화하는 LLC(Logical Link Control)와 MAC 주소를 사용하여 장치를 연결하고 데이터 송수신 권한을 정의하는 MAC(Media Access Control)의 두 부분으로 구성된다.
- 대표적 프로토콜: 이더넷, 토큰 링 / 디바이스:스위치
- 데이터 단위는 frame

### 3계층:  네트워크 계층

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/202c25fd-5d7e-4771-92c9-bf5e35fab220/Untitled.png)

- 네트워크 계층에는 두 가지 주요 기능이 있다.
    - 하나는 세그먼트를 네트워크 패킷으로 분할하고 수신 측에서 패킷을 재조립하는 것
    - 다른 하나는 물리적 네트워크에서 최상의 경로를 찾아 패킷을 라우팅하는 것
- 2홉 이상의 멀티 홉 통신 담당, 즉 실제 네트워크(host) 간에 데이터 라우팅을 담당한다.
    - 라우팅: 어떤 네트워크 안에서 통신 데이터를 짜여진 알고리즘에 의해 최대한 빠르게 보낼 최적의 경로를 선택하는 과정
- 네트워크 계층은 네트워크 주소(일반적으로 인터넷 프로토콜 주소)를 사용하여 패킷을 대상 노드로 라우팅한다.
- 라우팅, 흐름 제어, 세그멘테이션, 오류제어, 인터네트워킹
- 대표적 프로토콜: ip, icmp, ARP, RARP  / 디바이스: 라우터
- 데이터 단위는 packet

### 4 계층: 전송 계층

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/936bb6fa-9366-4f7f-a932-f8e55db4f975/Untitled.png)

- 전송 계층은 세션 계층에서 전송된 데이터를 전송 측에서 "세그먼트"로 나눈다.
- 수신 측에서 세그먼트를 재조립하여 세션 계층에서 사용할 수 있는 데이터로 되돌리는 역할을 한다.
- 전송 계층은 수신 장치의 연결 속도와 일치하는 속도로 데이터를 보내는 흐름 제어와 데이터가 잘못 수신되었는지 확인하고 그렇지 않은 경우 다시 요청하는 오류 제어를 수행한다.

- 양 끝단(End to end)의 사용자들이 신뢰성있는 데이터를 주고 받을 수 있도록 해주어 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 부담을 덜어준다.
    - 즉 전송 계층은 보내고자 하는 데이터의 용량과, 속도, 목적지(port)를 처리합니다.
    - 이때 시퀀스 넘버 기반의 오류 제어 방식을 사용한다.
- 전송 계층은 특정 연결의 유효성을 제어하고, 일부 프로토콜은 상태 개념이 있고(stateful), 연결 기반(connection oriented)이다.
    - 이는 전송 계층이 패킷들의 전송이 유효한지 확인하고 전송 실패한 패킷들을 다시 전송한다는 것을 뜻한다.
- 대표적 프로토콜 TCP, UDP / 디바이스: 게이트웨이
- 데이터 단위는 TCP-segment, UDP-datagram

### 5계층: 세션 계층

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d3e1ce11-26bb-455e-860f-f5e7f1f626c7/Untitled.png)

- 5계층에서 실제 네트워크 연결이 이뤄짐. 두 컴퓨터 간의 대화나 세션을 관리하며, 포트(Port)연결이라고도 한다.
- 동시 송수신 방식(duplex), 반이중 방식(half-duplex), 전이중 방식(Full Duplex)의 통신과 함께, 체크 포인팅과 유휴, 종료, 다시 시작 과정 등을 수행한다
- TCP/IP 세션을 만들고 없애는 책임을 진다.
- 대표적 프로토콜: NetBIOS, SAP, SDP, NWLink
- 데이터 단위 data

### 6계층: 표현 계층

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ae526e96-1867-4233-948b-c3149c5168a7/Untitled.png)

- 6계층은 응용프로그램 혹은 네트워크를 위해 데이터를 인코딩과 디코딩
- 코드 간의 번역을 담당하여 사용자 시스템에서 데이터의 형식상 차이를 다루는 부담을 응용 계층으로부터 덜어 준다.
- 대표적인 예로 암호화하고 복호화, mime 인코딩
- 대표적 프로토콜: ASCII, MPEG, JPEG, MIDI

### 7계층: 응용 계층

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0377632e-b1d2-41b9-8ab4-899f9357687e/Untitled.png)

- 사용자가 네트워크에 접근할 수 있도록 인터페이스를 제공하는 계층
- 즉 사용자가 실행하는 응용 프로그램들이 계층 7에 속한다고 보면 된다.
    - 텔넷, 크롬, 이메일, 터미널 등
- 대표적 프로토콜: HTTP, SMTP, FTP

# TCP/IP 4계층

![https://media.vlpt.us/images/inyong_pang/post/b6748747-d891-46e6-88cd-268f7497a40b/image.png](https://media.vlpt.us/images/inyong_pang/post/b6748747-d891-46e6-88cd-268f7497a40b/image.png)
![https://media.vlpt.us/images/inyong_pang/post/35109cfa-b496-4203-998a-bbf099a05387/image.png](https://media.vlpt.us/images/inyong_pang/post/35109cfa-b496-4203-998a-bbf099a05387/image.png)

- ARPANET이 개발된 이후 현재의 인터넷으로 발전해나가는 과정에서 대부분의 데이터 통신이 TCP와 IP기반으로 이루어졌기 때문에 인터넷 프로토콜 그 자체를 표현하는 용어
- 사실상 인터넷 프로토콜을 대표하는 용어로 사용
- TCP/IP는 현재 인터넷에서 컴퓨터들이 서로 정보를 주고받는데 쓰이는 통신규약(프로토콜)의 모음

## 1계층 - 네트워크 액세스 계층(Network Access Layer)

- OSI 7계층의 물리계층과 데이터 링크 계층에 해당
- 물리적인 주소로 MAC을 사용
- CSMA/CD, MAC, LAN, X25, 패킷망, 위성 통신, 다이얼 모뎀, LAN, 패킷망 등
- Ehternet(이더넷), Token Ring, PPP 등등

## 2계층 - 인터넷 계층(Internet Layer)

- OSI 7계층의 네트워크 계층에 해당
- 통신 노드 간의 IP패킷을 전송하는 기능과 라우팅 기능을 담당
- IP, ICMP, ARP, RARP, OSPF, BGP 등등

## 3계층 - 전송 계층(Transport Layer)

- OSI 7계층의 전송 계층에 해당
- 통신 노드 간의 연결을 제어하고, 신뢰성 있는 데이터를 전송
- TCP, UDP 등등

## 4계층 - 응용 계층(Application Layer)

- OSI 7계층의 세션 계층, 표현 계층, 응용 계층에 해당
- TCP/UDP 기반의 응용 프로그램을 구현할 때 사용
- SMTP, FTP, HTTP, SSH, DNS 등등

## 참고출처

- [https://velog.io/@inyong_pang/OSI-7-계층과-TCPIP-계층](https://velog.io/@inyong_pang/OSI-7-%EA%B3%84%EC%B8%B5%EA%B3%BC-TCPIP-%EA%B3%84%EC%B8%B5)
- [https://www.guru99.com/difference-tcp-ip-vs-osi-model.html](https://www.guru99.com/difference-tcp-ip-vs-osi-model.html)
