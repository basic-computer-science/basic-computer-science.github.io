---
layout: post
title:  "[Network] 3nd-Application Layer"
categories: Network
---

# 3주차 

---

# Application Layer

### 3-1. Web and HTTP
> HTTP란 무엇인가?

+ 인터넷에서 데이터를 주고받을 수 있는 프로토콜

+ TCP/IP 를 이용하는 프로토콜

+ 네트워크 계층 중 최상위 계층인 Application layer에서 동작하는 프로토콜 

+ 클라이언트(사용자)가 브라우저를 통해서 어떠한 서비스를 url등을 통해서 요청(request)을 하면 서버에서는 해당 요청사항에 맞는 결과를 찾아서 사용자에게 응답(response)하는 형태로 동작한다.

+ HTML을 전달하기 위한 프로토콜(JSON 데이터나 XML 같은 형태도 가능)

+ HTTP는 연결 상태를 유지하지 않는 비연결성 프로토콜이다.

> Stateless

+ 한번의 요청과 응답이 이루어지면 클라이언트와 서버와의 연결선이 끊어지며 클라이언트와 서버가 주고받았던 데이터들은 다음 요청 및 응답에 존재하지않는 상태

+ 어떤 학생이 똑같은 질문을 계속 하는데 계속 똑같이 기억못하고 대답해주는 것

+ Stateless 구조는 client와의 정보를 기억할 필요가 없으므로, 이러한 정보를 서버에 저장하지 않는다. 

+ 서버의 부하를 줄여준다는 장점이 있다.

+ 보안상으로 좋다.

> Non Persistent HTTP vs Persistent HTTP 

##### **Non Persistent HTTP**

+ Stateless로 동작하는 HTTP

+ Server와 Client가 통신하기 위해서는 TCP 연결이 필요한데, Stateless로 동작하면 매번 TCP 연결을 다시 해야한다.

+ 100번의 request를 보내면 100번의 tcp연결을 해야하고 100번의 response를 받아야 하고… -> tcp connection 하는 시간이 계속 걸린다

+ tcp 연결을 유지한다면 시간을 줄일 수 있기 때문에 Persistent HTTP 필요

##### **Persistent HTTP**
+ TCP 연결을 유지하는 방법

+ client와 해당 server 간의 단일 TCP 연결을 통해 여러 개체를 보낼 수 있다.

+ 보안상의 문제로 Non-persistent HTTP를 더욱 선호, 서비스 요구 사항에 따라 선택

> Proxy server

+ 서버와 클라이언트 사이에서 대리로 통신을 수행해주는 서버

+ 기존의 인터넷 철학은 stateless였는데, 상업적 인터넷은 state로 발전되면서 나온 개념(Cache의 역할을 한다.)

+ 원래 요청하려는 서버까지 가지 않고 빠르게 proxy server를 통해서 갈 수 있다. 동일한 정보를 얻을 때 오리지널 서버까지 갈 필요없이 프락시에 미리 저장해놓으면 훨씬 빠르게 처리할 수 있다.

+ 프록시 서버가 중간에 경유하게 되면 IP를 숨기는 것이 가능해져 보안 목적으로 사용하기도 한다.(프록시 방화벽)

+ Proxy server에 없는 경우는 오리지널 Server까지 가야한다.

![1](/public/img/network/캡처.JPG)


### 3-2.Electronic Mail
Application layer에서 네트워크를 이용한 프로그램들 중 전자 메일이 있다.

![2](/public/img/network/2.JPG)

**1. SMTP**
- Simple Mail Transfer Protocol의 줄임말, email 전송의 업계표준 프로토콜

**2. IMAP**
- SMTP는 이메일을 보내는 것이라고 했을 때 IMAP는 이메일을 받은 서버로부터 이메일 메시지를 관리하고 이메일을 꺼내서 가져오는데 사용되는 프로토콜

- Internet Access Message Protocol

**3. POP3**
- POP3는 IMAP과 마찬가지로 이메일을 받아오는 프로토콜이며 Post Office Protocol의 줄임말이다. 

- POP3는 서버로부터 이메일을 가져오고 가져온 메일이 확인되면 서버로부터 이메일을 삭제한다.(IMAP과의 차이점!IMAP은 반면에 서버에 메시지를 저장하고 여러가지 디바이스에 이 메일들을 동기화한다.) 


### 3-3.DNS
> What is DNS?

- Domain Name System의 약자로 인터넷 주소창에 Host Domain Name을 입력했을 때(ex, naver.com, google.com..) 해당 문자를 IP주소로 변환해 주는 시스템을 말한다.

> DNS 서버의 종류

**1. Local DNS Server**
 - URL에 Domain Name을 입력했을 때 해당 IP를 찾기위해 가장먼저 찾는 DNS서버
- 보통 통신사 DNS 서버로, 해당하는 도메인 주소의 IP가 있으면 알려주고 없다면 Root Server에게 물어본다.

**2. Authoritative DNS Server**
- 도메인 네임에 대한 정보를 갖고 있으면서, 해당 도메인 네임에 해당하는 IP주소를 갖고있는 서버

- 모두 다 알고 있는 서버이다.

**3. Root DNS Server**
- Root DNS는 최상위 DNS서버로 요청이 들어오면 해당 DNS부터 시작해서 아래로 연결된 node DNS서버로 차례차례 물어보게 된다.(분산화된 구조)

![3](/public/img/network/3.JPG)

- Local DNS Server에 도메인의 IP주소가 없다면
- client는 Root DNS에게 도메인 주소를 요청한다. 하지만 없다면
- Root DNS는 .com의 주소는 알고 있으니 .com DNS Server에게 물어보라고 한다.
- 위의 과정을 반복하다가
- client는 amazon.com DNS Server로부터 IP 주소를 얻게 된다.

**4. TLD DNS Server**
- .kr,.jp,.CN과 같은 국가 코드 최상위 도메인과 .com,.net과 같은 일반 최상위 도메인으로 구성

![4](/public/img/network/4.JPG)

> DNS가 분산화되어있는 이유

1. 중앙 서버까지의 거리가 멀기 때문에
2. 확장 가능성(끊임없이 확장할 수 있기 때문)
3. 유지보수
4. 안정성(네임서버 하나에 장애가 생겨도 다른 네임서버를 사용할 수 있으니 네트워크의 안정성이 높아짐)


> Iterated Query vs Recursive Query

**1. Iterated Query**

![5](/public/img/network/5.JPG)

1. Host가 naver.com에 대해 query를 보냄
2. Local DNS server가 root name server에 query를 보내 com담당 server의 주소를 return 받고, 다시 com 담당 server에 query를 보내 naver.com의 변환 요청을 보냄
3. 최종 IP 주소를 받을 때까지 요청과 응답을 계속해서 local name server가 반복하는 방법이다.

**2. Recursive Query**

![6](/public/img/network/6.JPG)

1. local Host가 naver.com에 대해 query를 보내면 Local DNS server가 root name server에 query를 보냄
2. root server는 자신의 server에 등록되어 있는지 검사한 다음 없으면 com 담당 서버에 요청을 한다.
3. 재귀적으로 실제 domain name을 가지고 있는 server까지 query가 이동하여 IP 주소를 얻는 방법

   단점 : root server에 너무 큰 부담을 준다는 단점이 있다.


### 면접 기출 질문

>> Q. DNS 프로토콜은 무엇인가? DNS 서버가 중앙화 되지 않는 이유는? 로컬 DNS 서버에 요청한 네임에 대한 매핑이 존재하지 않으면 어떻게 하는가? iterated query, recursive query 시나리오로 설명해보라.

>> Q. HTTP 프로토콜에 대하여 설명하고, Persistent와 Non-persistent 방식을 비교하라.

>> Q. SMTP에 대해 간단히 설명하라. POP3와 IMAP의 차이는 무엇인가?

*참고자료 

[https://velog.io/@surim014/HTTP%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80](https://velog.io/@surim014/HTTP%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)

[https://liveyourit.tistory.com/251](https://liveyourit.tistory.com/251)

[https://m.blog.naver.com/ijoos/221742035684](https://m.blog.naver.com/ijoos/221742035684)

[https://hwan-shell.tistory.com/320](https://hwan-shell.tistory.com/320)


