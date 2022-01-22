### 개요

- TCP는 송신자가 수신자의 버퍼를 오버플로 시키는 것을 방지하기 위해서 애플리케이션에게 흐름제어 서비스를 제공한다
- 수신하는 애플리케이션이 읽는 속도와 송신자가 전송하는 속도를 같게 한다.
- 흐름제어와 혼잡제어는 송신자의 억제로 동작이 비슷하지만, 명백히 서로 다른 목적을 위해 수행된다

## **TCP 통신이란?**

- 네트워크 통신에서 신뢰적인 연결방식
- TCP는 기본적으로 unreliable network에서, reliable network를 보장할 수 있도록 하는 프로토콜
- TCP는 network congestion avoidance algorithm을 사용
-

## **reliable network를 보장한다는 것은 4가지 문제점 존재**

1. **손실** : packet이 손실될 수 있는 문제
2. **순서 바뀜** : packet의 순서가 바뀌는 문제
3. **Congestion** : 네트워크가 혼잡한 문제
4. **Overload** : receiver가 overload 되는 문제

## **흐름제어/혼잡제어란?**

- **흐름제어** (endsystem 대 endsystem)
    - 송신측과 수신측의 데이터 처리 속도 차이를 해결하기 위한 기법
    - Flow Control은 receiver가 packet을 지나치게 많이 받지 않도록 조절하는 것
    - 기본 개념은 receiver가 sender에게 현재 자신의 상태를 feedback 한다는 점
- **혼잡제어** : 송신측의 데이터 전달과 네트워크의 데이터 처리 속도 차이를 해결하기 위한 기법

## 전**송의 전체 과정**

- Application layer : sender application layer가 socket에 data를 씀.
- Transport layer : data를 segment에 감싼다. 그리고 network layer에 넘겨줌.
- 그러면 아랫단에서 어쨋든 receiving node로 전송이 됨. 이 때, sender의 send buffer에 data를 저장하고, receiver는 receive buffer에 data를 저장함.
- application에서 준비가 되면 이 buffer에 있는 것을 읽기 시작함.
- 따라서 flow control의 핵심은 이 receiver buffer가 넘치지 않게 하는 것임.
- 따라서 receiver는 RWND(Receive WiNDow) : receive buffer의 남은 공간을 홍보함

## **흐름제어 (Flow Control)**

- 수신측이 송신측보다 데이터 처리 속도가 빠르면 문제없지만, 송신측의 속도가 빠를 경우 문제가 생긴다.
- 수신측에서 제한된 저장 용량을 초과한 이후에 도착하는 데이터는 손실 될 수 있으며, 만약 손실 된다면 불필요하게 응답과 데이터 전송이 송/수신 측 간에 빈번이 발생한다.
- 이러한 위험을 줄이기 위해 송신 측의 데이터 전송량을 수신측에 따라 조절해야한다.

## **해결방법**

- **Stop and Wait** : 매번 전송한 패킷에 대해 확인 응답을 받아야만 그 다음 패킷을 전송하는 방법

[https://t1.daumcdn.net/cfile/tistory/263B7D4E5715ECEB32](https://t1.daumcdn.net/cfile/tistory/263B7D4E5715ECEB32)

- **Sliding Window** (Go Back N ARQ)
    - 수신측에서 설정한 윈도우 크기만큼 송신측에서 확인응답없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어기법
    - 목적 : 전송은 되었지만, acked를 받지 못한 byte의 숫자를 파악하기 위해 사용하는 protocol

      LastByteSent - LastByteAcked <= ReceivecWindowAdvertised

      (마지막에 보내진 바이트 - 마지막에 확인된 바이트 <= 남아있는 공간) ==

      (현재 공중에 떠있는 패킷 수 <= sliding window)

    - **동작방식** : 먼저 윈도우에 포함되는 모든 패킷을 전송하고, 그 패킷들의 전달이 확인되는대로 이 윈도우를 옆으로 옮김으로써 그 다음 패킷들을 전송
    - **Window** : TCP/IP를 사용하는 모든 호스트들은 송신하기 위한 것과 수신하기 위한 2개의 Window를 가지고 있다. 호스트들은 실제 데이터를 보내기 전에 '3 way handshaking'을 통해 수신 호스트의 receive window size에 자신의 send window size를 맞추게 된다.

[https://t1.daumcdn.net/cfile/tistory/253F7E485715ED5F27](https://t1.daumcdn.net/cfile/tistory/253F7E485715ED5F27)

### **세부구조**

1. **송신 버퍼** - 200 이전의 바이트는 이미 전송되었고, 확인응답을 받은 상태 - 200 ~ 202 바이트는 전송되었으나 확인응답을 받지 못한 상태 - 203 ~ 211 바이트는 아직 전송이 되지 않은 상태

   [https://t1.daumcdn.net/cfile/tistory/22532F485715EDF218](https://t1.daumcdn.net/cfile/tistory/22532F485715EDF218)

2. **수신 윈도우**

   [https://t1.daumcdn.net/cfile/tistory/25403A485715EE362B](https://t1.daumcdn.net/cfile/tistory/25403A485715EE362B)

3. **송신 윈도우** - 수신 윈도우보다 작거나 같은 크기로 송신 윈도우를 지정하게되면 흐름제어가 가능하다.

   [https://t1.daumcdn.net/cfile/tistory/2520244B5715EE6A14](https://t1.daumcdn.net/cfile/tistory/2520244B5715EE6A14)

4. **송신 윈도우 이동**
    - Before : 203 ~ 204를 전송하면 수신측에서는 확인 응답 203을 보내고, 송신측은 이를 받아 after 상태와 같이 수신 윈도우를 203 ~ 209 범위로 이동
    - after : 205 ~ 209가 전송 가능한 상태

   [https://t1.daumcdn.net/cfile/tistory/227DC8505715EEBA0A](https://t1.daumcdn.net/cfile/tistory/227DC8505715EEBA0A)

5. Selected Repeat

---

## **Sliding Window** 흐름제어 조금 더 자세히

- TCP는 송신자가 수신 윈도우라는 변수를 유지하여 흐름제어를 제공
- 수신 윈도우는 수신 측에서 가용한 버퍼 공간이 얼마나 되는지를 송신자에게 알려 주는데 사용된다.
- TCP는 전이중이므로 각 측의 송신자는 별개의 수신 윈도우 유지
- example
    - 호스트 a → 호스트 b에게 큰 파일 전송 가정
    - 호스트 b는 이 연결에 수신 버퍼 할당, 이때 할당된 수신 버퍼의 크기를 RcvBuffer라고 명명
    - 시간 나는 대로 호스트 B의 애플리케이션 프로세스는 버퍼로부터 데이터를 읽으며 다음과 같은 변수들을 정의
        - LastByteRead: 호스트 b의 애플리케이션 프로세스에 의해서 버퍼로부터 읽힌 데이터 스트림의 마지막 바이트의 수
        - LastByteRcvd: 호스트 b에서 네트워크로부터 도착하여 수신 버퍼에 저장된 데이터 스트림의 마지막 바이트의 수
    - TCP는 버퍼 오버플로를 허용하지 않으므로 다음 수식 가능
        - LastByteRcvd - LastByteRead ≤ RcvBuffer
    - rwnd로 명명된 수신 윈도우는 버퍼의 여유 공간으로 설정
        - rwnd = RcvBuffer - [LastByteRcvd - LastByteRead]
    - 호스트 b는 호스트 b가 호스트 a에게 전송하는 모든 세그먼트의 윈도우 필드에 현재의 rwnd 값을 설정함으로써 연결 버퍼에 얼마만큼의 여유 공간이 있는지를 호스트 a에게 알려 준다
    - 반면 호스트 a는 LastByteSend, LastByteAcked를 유지한다
        - (LastByteSend - LastByteAcked)의 값은 호스트 a가 이 연결에 전송 확인응답이 안 된 데이터의 양이다
    - rwnd보다 작은 확인응답 데이터의 양을 유지함으로써 호스트 b의 수신 버퍼에 오버플로가 발생하지 않는다는 것을 확신한다.
- 이 대 rwnd = 0을 호스트 a에게 알리면 호스트 b에서 버퍼를 비우더라도 a에게 새로운 세그먼트를 보내지 않으므로 데이터 전송이 끊긴다.
    - 이러한 문제를 해결하기 위해 호스트 a가 호스트 b의 수신 윈도우가 0일 때, 1바이트 데이터로 세그먼트를 계속해서 전송하도록 요구
    - 이 세그먼트들은 수신자에 의해 긍정 확인응답
    - 결과적으로 버퍼는 비워지고 긍정 확인 응답은 0이 아닌 rwnd 값을 포함
