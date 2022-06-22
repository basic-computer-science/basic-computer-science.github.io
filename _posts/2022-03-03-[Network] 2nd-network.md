---
layout: post
title:  "[Network] 2nd-계층화된 구조"
categories: Network
---

# 2주차

# 네트워크 계층 구조

![Img](/public/img/network/2주차/1.png){: width="500px" .center}
<br>

-  5계층 : 사용자에게 서비스 제공에 대한 기능 담당 / 전자 우편,
원격 파일 접근 및 전송 등 / Data / 헤더를 추가하지 않음 / HTTP, FTP 등 사용
-  4계층 : 송신자 및 수신자의 프로세스 간 End-to-End 메시지 전달 기능 /
3계층까지는 올바른 순서대로 도착해서 정상적인 메세지로
조립되었는 지 알지 못하지만 4계층에서 보장할 수 있음 /
Segment (UDP라면 Datagram) 단위 / 세그먼트 헤더에 포트 번호<span style="color: red">*</span> 추가하여 전송 / TCP, UDP 사용
-  3계층 : Routing 기능 / Packet 단위 / IP 주소(논리 주소)
헤더에 추가하여 전송 / IP 사용
-  2계층 : Frame 단위 / 헤더에 물리주소<span style="color: red">*</span> 추가
-  1계층 : 비트 전송의 기능

> * 포트주소 : 현재 실행되고 있는 프로세스를 구분하는 주소
> * 물리 주소 : 노드 주소 정보 (단일 링크에 대한 주소, 2계층에서
사용하는 주소 정보) (MAC과 LLC)

## 왜 모듈화가 되어야 하는가 ?

각 계층들 사이에서는 인터페이스를 가지는데, 상위계층이 하위계층에게 특정 서비스를
요청하는 방식으로 동작한다. 
- 시스템을 기능별로 간단하게 재구성할 수 있다. 
- 모듈 사이에 인터페이스가 존재해서 독립적으로 작동하지만 상호 유기적이다. 
- 오류를 수정하거나 향상시켜야 하는 경우 해당 계층만 고려해도 된다.

## Encapsulation (캡슐화) & Decapsulation (역캡슐화)

- 캡슐화 : 하위계층으로 내려가면서 필요한 데이터를 추가해 나가는 것
- 역캡슐화 : 헤더를 제거하면서 Application layer까지 전달

![img](/public/img/network/2주차/2.png)

<hr>

드라이브 파일:
[https://drive.google.com/file/d/1ftYLLythlOzfHgjc9iTOthqELwK5Wa9g/view?usp=sharing](https://drive.google.com/file/d/1ftYLLythlOzfHgjc9iTOthqELwK5Wa9g/view?usp=sharing)
