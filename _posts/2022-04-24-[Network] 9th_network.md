---
layout: post
title:  "[Network] 9th-Network Layer"
categories: Network
---

### 9주차 Network Layer

---

#### 4-6.IPv4 vs IPv6

    IPv6이 생긴 이유 :
    IPv4 어드레스가 고갈될 것으로 예측되어, 훨씬 더 큰 IP주소 풀을 생성하여 인터넷의 지속적인 확장이 가능하도록 설계

   - 표현 방법 : 128비트체계로 구성되어 있으며, 16비트씩 8부분으로 나누어 콜론으로 구분하여 표현
     - ex) 2001:230:abcd:ffff:0000:0000:ffff:1111

- 체크섬 필드, fragmentation 기능 제거(라우터마다 체크섬 계산 비효율적, 오버헤드가 커지는 것 방지)

- IPv6이 보편화가 되기에 시간이 좀 걸려서 터널링 기법을 통해 IPv4와 IPv6을 혼용하여 사용

- 현재까지는 부족한 IPv4 주소체계를 NAT를 이용하여 버팀

> What is Tuneling?

![1](/public/img/network/9주차/1.JPG){: .center}

    IPv4를 사용하는 라우터 사이에서 IPv6 데이터가 IPv4 데이터그램안에 들어감으로써 IPv4 네트워크에서 이동되어지도록 IPv6을 IPv4로 캡슐화하는 방법

- 듀얼스택 라우터 : IPv6과 IPv4를 혼용하여 사용하는 라우터, IPv4 터널을 다른 듀얼 스택 라우터와 연결하여 데이터 전송, 듀얼 스택 라우터에서 IPv6을 IPv4로 캡슐화 시켜줌 

---

#### 4-7. ARP(Address Resolution Protocol, 3계층 프로토콜)

> What is ARP?

   같은 네트워크 대역에서 통신을 하기 위해 필요한 MAC 주소(해당 IP에 맞는 물리적인 주소)를 IP주소를 이용해서 알아오는 프로토콜

> 통신 과정

1. ARP는 목적지 MAC 주소를 모르는 상태이기때문에, MAC address를 00:00:00:00:00:00(6byte)으로 비워둔 뒤, 같은 대역에 있는 모든 장비에게 브로드캐스트

2. 이를 받은 모두는 decapsulation을 통해 2계층(브로드캐스트임을 알 수 있다.)을 열어본다.

3. 3계층도 열어보게 되고, 본인의 ip주소와 목적지 ip주소가 일치하지 않는다면 패킷을 discard, 그렇지 않으면 응답 프로토콜을 만든 뒤 본인의 mac주소를 넣어 유니캐스트로 원래노드로 전송

4. 이를 다시 받은 원래 노드는 응답받은 맥주소를 **ARP 캐시 테이블**에 등록
-> MAC주소를 알아온 뒤에 다음 통신이 시작됨. 최초 통신의 경우 한 번 실행된 다음 통신이 시작되는 것

---

#### 4-8. gateway vs router

|Network equipment|Router|Gateway|
|------|---|---|
|주요 기능|올바른 목적지 주소로 데이터 패킷을 전송시킨다.|서로 다른 프로토콜을 사용하는 두 네트워크를 연결하기 위해 사용하는 translator 기능|
|OSI 계층|주로 3계층(LAN과 WAN을 연결하는데 사용)|프로토콜이 다른 네트워크(Layer 4)를 연결할 때 사용|
|Protocol|하나의 프로토콜 사용|하나 이상의 프로토콜 사용| 
|장치|Hardward|Software|

* 요즘에는 라우터와 게이트웨이 기능이 같이 있는 장비를 많이 사용하는 추세

* 게이트웨이가 없다면 자신이 속한 로컬 네트워크에서만 통신이 가능

* 자신의 컴퓨터에서 목적지 네트워크에 도달하기까지 최소 한개 ~ 다수의 게이트웨이를 거쳐가야함


---

**참고link**

**0. IPv6**
 - [https://movefast.tistory.com/53](https://movefast.tistory.com/53)


**1. ARP 프로토콜**
 - [https://velog.io/@combi_jihoon/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-5-ARP-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C](https://velog.io/@combi_jihoon/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-5-ARP-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)

- [https://musclebear.tistory.com/12](https://musclebear.tistory.com/12)

**2. Gateway vs Router**

 - [https://puzzle-puzzle.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%9A%A9%EC%96%B4-%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9B%A8%EC%9D%B4-Gateway-%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9B%A8%EC%9D%B4%EC%99%80-%EB%9D%BC%EC%9A%B0%ED%84%B0-%EC%B0%A8%EC%9D%B4%EC%A0%90](https://puzzle-puzzle.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%9A%A9%EC%96%B4-%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9B%A8%EC%9D%B4-Gateway-%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9B%A8%EC%9D%B4%EC%99%80-%EB%9D%BC%EC%9A%B0%ED%84%B0-%EC%B0%A8%EC%9D%B4%EC%A0%90)