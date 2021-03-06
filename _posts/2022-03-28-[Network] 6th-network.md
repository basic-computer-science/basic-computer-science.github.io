---
layout: post
title:  "[Network] 6th-Transport Layer"
categories: Network
---

### 6주차

---

### Transport Layer

---

### 3-4. principles of rdt

> stop and wait의문제점

    데이터를 하나 보내고 ACK 신호를 기다릴 때까지 다음 데이터를 보내지 않아 성능 측면에서 문제가 있다.


> Pipelined protocols

![1](/public/img/network/6주차/1.JPG)

   파이프라인 프로토콜이란 ? Stop-and-Wait 방식과 다르게 송신자가 다수의 패킷을 한 번에 보내는 것을 의미(이를 구현하기 위해서 송신자, 수신자 모두 Buffer가 필요하다)

> 파이프라인 프로토콜의 종류

**1. Go-back-N**

![2](/public/img/network/6주차/2.JPG)

    Window size n : 버퍼의 사이즈를 의미함(window는 buffer)
    send_base : 버퍼의 시작점
    next_seq_num : 다음에 보낼 패킷 번호

- window size = 4

- 수신측 입장 

   - 패킷 0,1,2,3을 전송하는 도중에 2가 손실되었으면, 2에 대한 ACK가 아닌 가장 최신에 받은 pk인 1에 대한 ACK를 재전송한다.
   - 패킷 2번 이후에 받은 패킷들은 discard한다.

- 송신측 입장 

  - ACK 0 수신 : send_base를 한 칸 옆으로 이동시킨 뒤(0->1), window에 새로 들어온 패킷 4를 전송

  - ACK 1 수신 : send_base를 한 칸 옆으로 이동시킨 뒤(1->2), window에 새로 들어온 패킷 5 전송 
   
  - 중복된 ACK는 무시하게 되고 pk2번에 대한 timeout이 발생 -> 현재 윈도우 상에 있는 모든 패킷 재전송 (현시점 : 2,3,4,5)

   한계 : 하나만 손실되어도 window상에 있는(buffer안에 있는) 모든 패킷을 재전송해야한다.
 
**2. Selective Repeat**

![3](/public/img/network/6주차/3.JPG)

    송신측은 각 패킷마다 timer를 설정하고 timeout이 된 패킷만 재전송한다. 

- window size = 4

- 수신측 입장

   - 패킷 0,1,2,3을 전송하는 도중에 2가 손실 되었으면, GBN 방식과 달리 2번째 packet 이후에 오는 pk들을 수신측 버퍼에 보관해놓고 해당 pk에 대한 ACK를 보내준다. 
   - 그 후 pk2를 받았으면 ack2를 보낸 뒤 버퍼에 있는 누적된 pk을 상위 계층에 한꺼번에 전달한다.

- 송신측 입장

   - 제대로 된 ACK를 받았을 때는 GBN과 동작방식이 같음
   - packet 2에 대한 ACK를 받지 못하고 다음 pk에 대한 ack를 받았다면 기록만 해놓고 pk2가 오지않았으므로 send_base를 이동시키지 않는다.
   - 해당 pk에 대한 timeout이 된 경우 해당 pk(현재 pk2)만 재전송한다. 

   한계 : 모든 패킷에 timer가 있기 때문에 이에 대한 overhead가 크다.


> SR dilemma

![4](/public/img/network/6주차/4.JPG)

   만약 ACK가 모두 손실된 경우라면, 수신측 rcv_base는 packet을 모두 받았으므로, send_base를 이동시켜서 패킷 0이 window에 다시 들어가게 된다. 하지만 송신측 입장에서는 ACK가 오지 않아 timeout이 발생했으므로 기존의 pk0번을 다시 보내는데 수신측입장에서는 seq 번호만 보고 중복된 값이지만 중복된 pk0을 버퍼에 넣을 것이다. 

이런 문제를 해결하기 위해 SR은
    
    window size가 sequence number의 개수의 절반보다 이하여야한다. 

    window size <= (sequence number)/2


### 3-5. connection-oriented transport : TCP

> TCP Overview

![5](/public/img/network/6주차/5.JPG)
 
1. point-to-point : 한 명의 송신측과 하나의 수신측이 통신하는 1:1 방식
2. byte stream 방식 : 패킷의 boundary가 없는 전송 방식
3. pipelined : flow 제어와 congestion 제어에서 window size를 설정함으로써 pipelined
4. full duplex data : 송신측과 수신측 모두 데이터를 전송할 수 있는 양방향 통신
5. connection-oriented : 3-way handshaking을 통해 data를 주고 받기 전에 연결 상태로 유지
6. flow control : 흐름 제어 
7. congestion control : 혼잡 제어


> TCP reliable data transfer

![6](/public/img/network/6주차/6.JPG)

- TCP는 Tcp header(checksum,에러탐지)와 cumulative acks, retransmission(timer)을 사용해 패킷의 에러와 유실을 막아 rdt를 구현한다.

-  cumulative acks 사용

   누적된 ack를 사용함으로써 송신측에서 그 전 ack를 못 받았어도, 다음 pk을 전송한다. 

![7](/public/img/network/6주차/7.JPG)

- TCP fast retransmit

   timeout이 되지 않았더라도 송신측 입장에서 "triple duplicate ACKs"를 받으면 특정 seq 번호의 패킷을 재전송한다. 


> TCP connection management

>> 3-way handshaking

**1. 2-way handshaking**

![8](/public/img/network/6주차/8.JPG)

![9](/public/img/network/6주차/9.JPG)
   
한계
   
    half open connection : client가 ESTAB되기 전에 연결 요청을 다시 보내게 되면 연결이 맺어진 뒤에 다시 server에 연결요청이 가게 되어 client 입장에서는 완료되었는데 서버만 열리게 되는 현상

**2. 3-way handshaking**

![10](/public/img/network/6주차/10.JPG)

Step 1. client는 연결을 요청하는 SYNbit와 seq number를 보낸다.

Step2. server는 SYN요청을 수락한 뒤, client에게 요청을 수락한다는 SYN, ACK, seqnum을 보낸다.

Step3. client는 ESTAB상태가 되고, ACK를 보내주어 server도 ESTAB 상태가 되게 한다.

**3. 4-way handshaking**

![11](/public/img/network/6주차/11.JPG)

   FIN 플래그과 ACK를 이용하여 연결을 종료한다.




**기출 문제**

Q. Pipelined Protocol: Go-Back-N, Selective Repeat에 대해 간단히 설명하라.

Q. TCP의 3-way handshake (연결 성립, 연결 종료) 를 설명하라. 왜 3-way handshake를 사용하는가?

기출+: 3-way handshaking이란?


**참고 link**

[https://code-lab1.tistory.com/27](https://code-lab1.tistory.com/27)

[https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake](https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake)

