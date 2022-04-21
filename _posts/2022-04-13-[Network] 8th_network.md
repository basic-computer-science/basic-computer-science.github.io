---
layout: post
title:  "[Network] 8th-Network Layer"
categories: Network
---

### 8주차 Network Layer


#### 4-1. Routing and Forwarding

    Router가 가지고 있는 패킷을 전송하는 기능으로, 네트워크 레이어에서 동작하는 대표적인 기능이다. 

> Routing
 
 Source to Destination의 경로를 결정하는 것(라우팅 알고리즘을 통해 Forwarding table을 만드는 작업)

> Forwarding

![1](/public/img/network/8주차/1.JPG){: .center}

 각각의 Router에서 Forwarding table을 보고 입력포트에서 출력포트로 패킷을 내보내는 것 

 목적지의 주소(IP Address)와 출력링크의 최소 두 가지의 정보가 들어있음.


> Longest Prefix Matching

![2](/public/img/network/8주차/2.JPG){: .center}

    패킷 경로를 정하기 위해 라우터에서 패킷을 forwarding해줄때, 패킷의 목적지 주소와 라우팅 테이블 엔트리의 prefix를 비교해서 가장 많이 matching되는 출력포트로 나간다.


#### 4-2. IP Addressing

> subnets

![3](/public/img/network/8주차/3.JPG){: .center}

- What is subnet?

![7](/public/img/network/8주차/7.JPG){: .center}

   - IP주소를 지역적으로 나누어 여러개의 서로 연결된 지역 네트워크로 사용하는 방법

   - 클래스별로 네트워크 영역을 다르게 하는 방법

   - Subnet 부분을 네트워크 영역, 그 뒷부분을 호스트 영역라고 함

   - IP주소에서 남는 Host 주소를 유연하게 줄이거나 할 필요가 있기 때문에 CIDR을 사용한다.

- Why using?
   
   1. 크기가 큰 Network의 경우 한 Host에서 악성 코드가 감염될 경우 전체 Network에 피해를 줄 수 있으므로 네트워크를 지역적으로 분할
   2. 한 네트워크에 수 많은 호스트가 있을 경우 통신 저하

> CIDR

![4](/public/img/network/8주차/4.JPG){: .center}

- 클래스 없는 도메인 간 라우팅 기법

- 서브넷 마스크를 직접 정의함으로써(slash 뒤에) 네트워크 영역과 호스트영역을 가변적으로 조절 가능

-  [https://daengsik.tistory.com/8](https://daengsik.tistory.com/8)

- 각 클래스가 가지는 디폴트 서브넷 마스크에 얽매이지 않고 기존 IP 주소 할당 방식이었던 클래스를 대체하며 IP주소의 네트워크 영역, 호스트영역을 유연하게 나누어준다

- 기존은 클래스의 형태로 네트워크를 할당하여서 낭비되는 주소가 많았는데, IPv4 주소를 보다 효율적으로 사용

#### 4-3. DHCP(Dynamic Host Configuration Protocol)

![5](/public/img/network/8주차/5.JPG){: .center}

- 클라이언트/서버 모델이며, Host가 IP address를 할당받는 방법

- DHCP지원 클라이언트는 네트워크 부팅과정에서 DHCP서버에 IP주소를 요청하고 이를 얻을 수 있다 -> 인터넷 사용이 가능해짐

> 임대 과정

**1. DHCP Discover**

- DHCP server가 어디있는지 모르는 상태에서 동일 subnet상에 브로드캐스팅을 하여 DHCP 서버를 찾는다. 

- 동일 서브넷안에 있는 모든 단말들은 이 메세지를 수신한다.

**2. DHCP Offer**
  
- 자신의 존재(자신의 IP주소)를 알림과 동시에, 사용할 수 있는 IP 주소 정보를 포함한 다양한 네트워크 정보를 client에게 전달한다. 

- DHCP client가 보낸 broadcast flag 값에 따라 브로드 캐스팅을 할 수도, 유니 캐스트를 할 수도 있다.

**3. DHCP Request**

- DHCP 메세지를 통해 server의 존재를 확인하고 하나의 DHCP server를 선택한 뒤, 단말이 사용할 네트워크 정보를 요청한다. (DHCP Offer가 여러대에서 왔을 수 있음으로 선택할 DHCP 주소를 메세지에 지정)


**4. DHCP Ack**

- DHCP 서버는 Request내에 기록된 IP주소가 자신의 주소인지 확인한 후 단말에게 사용할 네트워크 정보 전달

- 이 경우 또한, Broadcast일수도 unicast일수도 있음

> 주소 임대기간 연장 과정

**1. DHCP Request**

- 임대기간 연장을 요청하는 Request를 Unicast로 client ip주소를 포함하여 전송

- "ip주소 x.x.x.x 임대기간을 연장하고 싶은데 허락해주세요!"

**2. DHCP Ack**

- 요청 수락 메세지를 모든 정보를 포함해서 Unicasting으로 보낸다.

- "요청을 수락하겠습니다. x.x.x.x ip주소를 ~초 더 사용하도록 하세요"


> 한계 
   
     DHCP 서버에 의존되기 때문에 서버가 다운 시, IP 할당이 제대로 이루어지지 않는다.

#### 4-4. NAT(Network Addressing Translation)

![6](/public/img/network/8주차/6.JPG){: .center}

- NAT는 IPv4의 주소 부족 문제를 해결하기 위해, 비공인 네트워크 주소를 사용하는 망에서 공인 IP와의 통신을 위해 주소를 변환하는 방법

- IP 패킷의 TCP/UDP 포트 번호와 소스 및 목적지의 IP 주소 등을 재기록

- 공인 IP를 절약할 수 있고, 외부 침입자로부터 보호할 수 있다.

- 단점 : 
   1. 추가적인 시스템이기 때문에 복잡성을 증가시킨다.
   2. 호환성 문제 : NAT는 IP헤더 필드만을 수정하고, 데이터 영역은 수정하지 않기 때문에 특정 애플리케이션에 호환성 문제를 야기 시킬 수 있다.
   3. 네트워크 성능 지연 : 모든 데이터에 대하여 주소 변환을 하면 네트워크가 지연된다.


**기출**

Q. NAT (Networking Address Translation)에 대해 설명하라. NAT의 문제점은 무엇인가?

Q. DHCP 프로토콜에 대해 설명하라. DHCP 서버의 역할은 무엇인가? 호스트는 어떻게 DHCP 서버와 통신하게 되는가?


**참고 링크**

1. 라우팅과 포워딩
   [https://nenunena.tistory.com/52](https://nenunena.tistory.com/52)

2. 서브넷 & CIDR
   [https://kibua20.tistory.com/132](https://kibua20.tistory.com/132)

   [https://hwannny.tistory.com/86](https://hwannny.tistory.com/86)

3. NAT
[https://velog.io/@hidaehyunlee/%EA%B3%B5%EC%9D%B8Public-%EC%82%AC%EC%84%A4Private-IP%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90](https://velog.io/@hidaehyunlee/%EA%B3%B5%EC%9D%B8Public-%EC%82%AC%EC%84%A4Private-IP%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)

   [https://jwprogramming.tistory.com/30](https://jwprogramming.tistory.com/30)

4. DHCP

   [https://ja-gamma.tistory.com/entry/DHCP%EA%B0%9C%EB%85%90%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC](https://ja-gamma.tistory.com/entry/DHCP%EA%B0%9C%EB%85%90%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC)

   [https://jwprogramming.tistory.com/35](https://jwprogramming.tistory.com/35)