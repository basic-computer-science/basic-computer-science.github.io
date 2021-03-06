---
layout: post
title:  "[Network] 4th-Application Layer"
categories: Network
---

# 4주차 

---

# Application Layer

### 3-4. P2P application 

Application layer에는 두 가지 구조가 있다. 

**1. client-server 구조**

- client-server 구조는 client와 server 간에 데이터를 주고받는 형태이다. 
  - client : 데이터를 요청해서 받음
  - server : 데이터를 보내는 쪽이다.

- client 끼리는 직접적으로 통신하지 못하고, server를 통해서 데이터를 주고 받는 구조

**2. P2P 구조**

- 서로 동등한 Peers(종단 시스템)끼리  직접적으로 통신을 해서 정보를 주고 받는다. 

- 새로운 peers가 나타나면 스스로 규모가 늘어난다. P2P link가 많아지는데, 이를 self scalability라고 한다.

- 양방향 파일 전송 시스템이며, 각각의 컴퓨터가 서버와 클라이언트 역할을 하고, 서로 연결하여 파일을 주고받는 것

>  BitTorrent

- P2P방식으로 동작하는 프로그램

- 파일을 256KB의 청크(단위)로 쪼갠 뒤, 각 peer들은 자신이 가지고 있는 청크를 다른 peer들에게 업로드함과 동시에 다운로드하는 방식이다.(다운로드를 하면서 바로 업로드를 하기에 굉장히 효율적인 방법)

- selfish peer : 다운로드만 받고 업로드는 하지 않는 peer

- tit-for-tat 원칙 : 
1. 빠른 다운로드를 위해 좋은 peer를 남기고 selfish peer를 버리기 위한 전략
2. 청크를 많이 가진 top4 peer를 선정해 서로 공유
3. 업로드를 많이 한 Peer일수록 다운로드에 우선순위를 부여함으로써 서로 공유하기에 매끄러운 환경을 만들어 준다.

- rare-in-first : Peer들이 적게 가지고 있는 청크일수록 우선순위를 두고 전송. 청크의 복사본이 늘어나 파일 공유가 훨씬 수월해지기 때문

# 3-5.Socket Programming with UDP and TCP

> Socket이란?

![1](/public/img/network/4주차/1.JPG)

- Application 계층과 Transport 계층 사이에 있는 인터페이스

- Application layer에서 동작하는 프로세스가 데이터를 보내거나 받기 위해서는 반드시 소켓(바구니,문)을 열어 데이터를 전송하거나 소켓으로부터 데이터를 읽어들어야한다.

- 송신자는 Transport layer에서 소켓으로부터 데이터를 받아 세크먼트화하여 수신자에게 전송한다.

- 수신자는 Transport layer에서 온 데이터를 적절한 소켓으로 전달한다.(각각의 프로세스로 적절한 메세지를 전송하기 위해 port번호를 사용한다.)

- 소켓은 프로토콜, IP 주소, 포트 넘버를 할당 받는다.


> TCP 소켓 vs UDP 소켓

**TCP**

1. 양방향 전송, 연결 지향성-> 신뢰적인 프로토콜

2. 세그먼트가 순서대로 정확하게 전달되도록 한다. 

3. 오류 수정, 흐름 제어 보장

**UDP**

1. 비연결형 소켓

2. 데이터 크기에 제한이 있다. 

3. 확실하게 전달이 보장되지 않는다.

4. 그럼 왜 사용하나? 실시간 멀티미디어 정보를 처리할 때 주로 사용한다.(속도)

(-> 5주차에서 자세하게 설명)

# 3-6. video streaming and CDNs

> What are CDNs?

- 지리, 물리적으로 떨어져 있는 사용자에게 컨텐츠를 더 빠르게 제공할 수 있는 기술

- 느린 응답속도와 다운로딩 타임을 극복하기 위한 기술이다.

- 사용자가 원격지에 있는 Origin Server로 부터 이미지, 비디오, 음악 등과 같은 Contents를 받을때 가까이 있는 서버에서 받는 것보다 시간이 오래 걸리므로 사용자와 가까운 곳에 위치한 Cache Server에 해당 Content를 캐싱하고 Content 요청시에 Cache Server가 응답을 주는 기술이다.


### 기출 문제
> 기출 문제 
Q. 단일 서버가 N개의 클라이언트에게 크기가 F인 파일을 전송하려고 한다. 서버의 업로드 capacity가 u, 클라이언트의 다운로드 capacity가 v라고 하면, 클라이언트-서버 구조에서의 파일 전송 완료에 걸리는 시간, P2P 구조에서의 걸리는 시간을 정의해보라.

- [link] [https://ddongwon.tistory.com/75](https://ddongwon.tistory.com/75)

- TCP,UDP 관련 기출은 Transport Layer에서!

### 참고자료 

[https://developyo.tistory.com/248](https://developyo.tistory.com/248)

[https://goddaehee.tistory.com/173](https://goddaehee.tistory.com/173)

