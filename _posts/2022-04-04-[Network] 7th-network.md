---
layout: post
title:  "[Network] 7th-Transport Layer(final)"
categories: Network
---

### 7주차 Transport Layer


#### 3-6. Flow Control 

> 흐름제어란?

  - receiver와 sender의 데이터 처리 속도 차이를 해결하기 위한 기법
  - IF,송신측의 전송량 > 수신측의 처리량 :
   packet loss의 위험이 있기 때문에 송신측의 패킷 전송량을 제어하게 된다. 
  - rwnd(Received Window size)에 따라 Sender는 데이터 양을 조절한다. 
  - receive buffer가 overflow 되지 않게 보장하는 기법 


#### 3-7. Congestion Control 

> 혼잡제어란?

- 공유자원인 네트워크망의 혼잡을 약화시켜 통신에 충돌이 나게 하는 것을 줄이고, 한정된 자원을 잘 분배하여 원활히 돌아갈 수 있도록 제어하는 것이다.

- Router에 있는 큐는 유한하기 때문에 공유하고 있는 버퍼에 패킷이 꽉 차게 되면 패킷 Delay가 발생하고 패킷 Loss가 발생하게 된다. 이렇기에 혼잡 제어가 필요하다. 

> cwnd ? 

   - 송신측 윈도우를 계산하기 위한 변수 중 하나로, 단위는 MSS(Maximum Segment Size)
   - Sender가 가지고 있는 윈도우로, 송신 윈도우가 아님.
   - 혼잡 정책에 의해 증가되고 감소된다.
   - sender는 최종 송신 윈도우 사이즈를 정할 때 
    
    min(RWND, CWND)
    *RWND : flow control을 보고 판단
    *CWND : congestion의 정도를 보고 정한 윈도우 크기 

> Two Send Method

   TCP에서는 대표적인 혼잡 회피 방법인 AIMD와 Slow Start 방법을 조합해서 혼잡 제어 정책을 수행한다. 

**1. AIMD**

![1](/public/img/network/7주차/1.JPG)


 - 합 증가 / 곱 감소 방식으로 늘어날 때는 1씩 늘어나고, 혼잡 상태가(데이터가 유실되거나 응답이 오지않는 등) 감지되어 줄어들때는 절반으로 확 줄어드는 방식

**2. Slow Start**

![2](/public/img/network/7주차/2.JPG)

- 하나씩 올리는 AIMD의 단점을 보완하기 위해, 혼잡이 감지될 때까지 지수승으로 올리는 방법
- 혼잡이 감지되면 cwnd를 1로 줄인다. 

> TCP 혼잡 정책

   - 최근까지 Tahoe, Reno, New Reno, Cubic, 최근 나온 Elastic-TCP 등등 다양한 혼잡제어 존재  
   - 이러한 혼잡 제어 정책들은 공통적으로 혼잡이 발생하면 윈도우 크기를 줄이거나, 혹은 증가시키지 않으며 혼잡을 회피한다라는 전제

   - 임계점(ssthresh)을 사용하는 이유 : slow start로 계속 지수승으로 증가하다 보면 제어가 힘들기도 하고, 혼잡 예상 상황에서는 AIMD로 전환하는 것이 훨씬 안전하기 때문

**1. TCP Tahoe**

![3](/public/img/network/7주차/3.JPG)

- 첫 단계 : Slow Start
   - IF, 임계점을 만나면? AIMD 방식으로 교체
- IF, 혼잡 감지 되는 상황이 오면?
   - 3 duplicated ACKs인 경우 : cwnd를 1로 줄인 후, 임계점을 loss가 발생한 지점의 절반으로 줄임, 다시 slow start 시작
   - timeout의 경우 : 위와 같음


**2. TCP Reno**

![4](/public/img/network/7주차/4.JPG)

- TCP Tahoe와의 차이점 : 혼잡의 상태가 발생했을때, action을 구분한다(time out / duplicated ACKs)

- 첫 단계 : Slow Start
   - IF, threshold를 만나면? AIMD 방식으로 교체

- IF, 혼잡 감지되는 상황이 오면?
   - 3 duplicated ACKs의 경우 : cwnd를 1로 줄이는 것이 아니라 AIMD처럼 반으로만 줄이고 임계점도 반으로 줄인다. 그 후 linear하게 증가(원래 cwnd에 빠르게 다시 도달할 수 있는 상태가 되어 Fast Recovery 상태라고 한다!!!)
   - Time out의 경우 : cwnd를 1로 줄이고, slow start상태로 가며 threshold는 변경하지 않는다.

   -> 혼잡에 경중을 따지는 정책


**링크**

[https://roka88.dev/114](https://roka88.dev/114)

[https://evan-moon.github.io/2019/11/26/tcp-congestion-control/](https://evan-moon.github.io/2019/11/26/tcp-congestion-control/)