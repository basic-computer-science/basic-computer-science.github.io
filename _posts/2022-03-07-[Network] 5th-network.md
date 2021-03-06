---
layout: post
title:  "[Network] 5th-Transport Layer"
categories: Network
---

### 5주차

---

### Transport Layer

#### Outline
![1](/public/img/network/5주차/1.JPG)


### 3-1. Transport layer service

- Transport 레이어에서는 다른 host에 존재하는 process들 끼리의 logical communication을 담당한다. 

- logical communication이란? 
 
  두 hosts사이엔 수 많은 router와 links들이 존재해있을 것이기때문에, direct한 연결이라고 볼 수 없다. 따라서 Transport 레이어는 logical적으로 coomunication 한다고 한다. 

- 이 레이어는 Application layer와 Network layer의 내부 소통을 위한 중간다리 역할을 해준다고 말할 수 있다. 

- 그렇다면, A host와 B host사이에 X Y Z 라는 라우터들이 있다면, X Y Z는 Transport layer를 가지고 있을까?
 
   그렇지 않다. 어떤 우편이 보내진다고 가정하고, 그 우편을 열거나 포장하는 역할이 Transport 레이어의 역할이라고 하면, 중간 전달자인 우체부들은 편지를 열어볼 수 없어야하며, 열어볼 필요도 없다. 따라서 해당 역할을 위한 Transport 레이어는 라우터에게 필요 없는 레이어다.

- Tansport Layer는 Segment라는 data type을 사용

> Protocol

**1. TCP**

-  TCP는 장치들 사이에 논리적인 접속을 establish하기 위하여 3 way handshaking 이라는 과정을 통해 연결을 설정하여 신뢰성을 보장하는 서비스

- TCP는 네트워크에 연결된 컴퓨터에서 실행되는 프로그램 간에 일련의 세그먼트라는 블록 단위를 안정적으로, 순서대로, 에러없이 교환할 수 있게 한다.

**2. UDP**
 
- 비연결형 서비스를 지원하는 전송계층 프로토콜로써, 인터넷상에서 서로 정보를 주고받을 때 정보를 보낸다는 신호나 받는다는 신호 절차를
거치지 않고,보내는 쪽에서 일방적으로 데이터를 전달하는 통신 프로토콜

- 데이터를 데이터그램 단위로 처리하는 프로토콜

- 안정성을 보장하진 않지만, 빠른 속도가 장점-> 비디오 서비스 이용할 때 주로 쓰임

![8](/public/img/network/5주차/8.JPG)

### 3-2. 다중화와 역다중화

**1. 역다중화**

- 트랜트포트 계층에 상대 프로세스로부터 수신된 세그먼트를 설정된 트랜스포트 프로토콜에 따라서 알맞게 검사가 된다.

- 검사가 완료된 세그먼트 집합은 올바른 소켓으로 전달된다. 이를 역다중화 라고 한다. 트랜스포트 계층 세그먼트를 포트번호와 헤더를 보고 적절한 소켓에 전달하는 과정이다.

**2. 다중화**

- 애플리케이션 계층에서 메시지를 만들고 이들은 소켓을 통해 트랜스포트 계층으로 넘어간다. 데이터는 세그먼트로 캡슐화 되고 이를 네트워크 계층으로 전달하는 작업을 다중화 라고 한다. 

- 역다중화와 다중화를 애플리케이션 계층과 트랜스포트 계층 사이뿐만 아니라 한 계층의 프로토콜이 상위 계층의 프로토콜에 의해 사용될 때마다 필요한 작업이다.

> TCP vs UDP

**1. TCP**

![4](/public/img/network/5주차/4.JPG)

- TCP 소켓은 출발지 IP주소, 포트번호, 목적지 IP주소, 포트번호 총 4가지 정보가 필요하다.


**2. UDP** 

![3](/public/img/network/5주차/3.JPG)

- UDP 소켓은 출발지 포트번호, 목적지 포트번호 값을 포함하는 세그먼트 사용

- UDP 소켓은 목적지 IP 주소와 목적지 포트번호에 의해서만 식별된다. 

- 만약 2개의 UDP 세그먼트들이 출발지 IP주소 또는 출발지 포트번호가 다르지만 동일한 목적지에 도착한다면, 이는 프로세스가 데이터를 받았지만 알고, 누구의 데이터인지 확인할 방법이 없다. (UDP가 왜 단방향 통신의 특성을 가지는지에 대한 이유가 된다.)


### 3-3. Connectionless Transport : UDP

- Error 정정 능력은 없으나 error 검출 능력은 있다. 

![5](/public/img/network/5주차/5.JPG)


### 3-4. Principles of reliable data transfer

> rdt란 ?

- RDT는 신뢰성 있는 데이터 교환을 의미한다. 즉 송/수신하는 데이터가 오류 없이 온전히 전송되는 것을 뜻한다.

-  Transport Layer(전송계층)에서는 신뢰성 있는 데이터 교환을 하고 싶어 하지만, 하위 레이어들에서는 신뢰성을 보장할 수 없기 때문에 문제가 발생할 수 있다. 이를 해결하기 위해 Transport Layer에서 RDT 프로토콜을 이용할 수 있다.



> rdt의 역사 

1. rdt1.0
   
   완벽하게 신뢰적인 채널 상에서의 신뢰적인 데이터 전송으로 완벽해서 피드백 할 필요 X

2. rdt 2.0 
   
   비트 오류가 있는 채널을 가정한 rdt
-> checksum이 필요하다. rdt1.0에 에러 탐지 기능이 추가된 것

- 그렇다면 에러 복구는 어떻게 할까?
  - ACKs :수신자가 송신자한테 명시적으로 패킷(pkt)을 잘 받았다고 말한다.
  - negative acknowledgement(NAKs): 수신자가 송신자한테 pkt에 에러 있다고 말한다.

   한계 : 어떤 패킷을 다시 보내야하는 지 몰라서 재전송 할 수 있고, 마지막으로 전송된 ACK/NAK이 송신자에게 정확하게 수신되었는지 알 수 없다.

   해결책 : 데이터 패킷에 순서 번호(seq,0과1로 구성)를 삽입 -> 수신자는 수신된 패킷의 순서 번호를 확인하여 재전송인지 판별 가능

2-1. rdt 2.1 

- 패킷에 순서번호를 추가해 rdt 2.0의 문제점 해결 

- 순서 번호는 패킷의 중복 재전송을 막기 위해 부여하는 것이므로 중복인지 아닌지 판단하는 데에는 0과 1 이면 충분하기에 순서번호는 0과 1만 존재 

  - 0번 패킷에 오류가 있다면 NAK 0 신호를 보내고 0번 패킷이 오길 기다린다.

  - 0번 패킷에 오류가 없다면 ACK 0 신호를 보내고 1번 패킷이 오길 기다린다.

  - 1번 패킷에 오류가 있다면 NAK 1 신호를 보내고 1번 패킷이 오길 기다린다.

  - 1번 패킷에 오류가 없다면 ACK 1 신호를 보내고 1번 패킷이 오길 기다린다. 

2-2. rdt 2.2(Nak-free-protocol)

- rdt 2.2는 rdt 2.1과 같지만,rdt 2.2는 NAK 대신 ACK 만을 사용한다는 점이 다르다.

  - 중복되는 ACK 신호를 받으면 현재 패킷을 다시 재전송하면 된다. 

  - 예를 들어 0번 패킷을 보내고 제대로 송신되어서 ACK 0를 받았다고 하자. 
  - 이후 1번 패킷을 보냈는데 수신 측에서 오류를 탐지했다면, ACK 1이 아닌 가장 최근에 전송에 성공한 ACK 0를 보낸다.

  - 수신 측은 ACK 1을 기대했으나 ACK 0를 중복으로 받았으므로 오류가 발생했다는 사실을 알고 1번 패킷을 재전송한다.

   한계 : ACK/NAK가 유실되었을 경우, 신호를 계속 기다리면서 다음 패킷을 보내지 못함

3. rdt 3.0

- Time out을 설정하여 합리적인 시간동안 기다리고, 신호가 도착하지 않으면 패킷을 재전송 

![6](/public/img/network/5주차/6.JPG)

- (b)는 패킷이 손실된 경우 -> timeout 발생하여 재전송 

![7](/public/img/network/5주차/7.JPG)

- (c)는 ACK가 손실되었을 경우
  -  (b)와 다른 점은 수신 측이 패킷1을 중복으로 받았다는 점
   수신측은 packet1을 중복하여 받았기 때문에 패킷을 버리고 ACK1을 다시 보내면 되기에 문제가 생기지 않는다.

- (d)는 ACK 신호 전달이 지연되었을 경우 
  - 계속해서 중복으로 패킷과 ACK 신호가 재전송되는 문제가 발생한다. 

**문제점**

1. ACK 신호가 지연된다면 위와 같이 전송이 꼬이는 문제가 발생한다. 

2. 데이터를 하나 보내고 ACK 신호를 기다릴 때까지 다음 데이터를 보내지 않아 성능 측면에서 문제가 있다.


**참고 link**

[https://devfallingstar.tistory.com/31](https://devfallingstar.tistory.com/31)

[https://code-lab1.tistory.com/26](https://code-lab1.tistory.com/26)

[https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4](https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4)

[https://mangkyu.tistory.com/15](https://mangkyu.tistory.com/15)


**기출문제**

Q. RDT(Reliable Data Transfer)에 대해 간단히 설명하라.

기출+: TCP와 같이 protocol을 reliable하게 설계 하려면 무엇이 필요한가?


Q. TCP와 UDP의 차이는 무엇인가? 대표적으로 어떤 곳에 쓰이는가?

기출+: TCP와 UDP의 차이점은?


