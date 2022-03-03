---
layout: post
title:  "Process & Thread"

categories: OS
---

# Process & Thread

# Questions

- process 와 thread 의 차이점은?
    
    메모리 구조의 차이점
    
    그에 따른 통신 방법의 차이점, context switch를 할때 차이점
    
- multi-process와 multi-thread 의 차이점은?
    
    multi-thread는 multiprocess보다 적은 메모리공간을 차지하고 context switching 이 빠른 장점이 있는 반면 synchronization 문제와 thread간의 의존성이 있어 하나가 문제가 생기면 전체적으로 문제가 발생할 가능성이 있음
    
    multi-process 는 각 프로세스가 각자의 자원을 독립적으로 가지고 있으므로 하나의 프로세스가 문제가 생기더라도 다른 프로세스에 영향을 안미친다는 장점이 있는 반면 multi-thread에 비해 많은 리소스를 사용한다.
    
- PCB 란?
    
    process control block 의 줄임말로 process가 생성될때마다 생기는 information의 모음
    
    구성요소로는 PID, state, register정보, PC, process우선순위, 할당된 메모리정보, 할당된 cpu time, 입출력 상태정보 등이 있다. 
    
- process 간의, thread간의 통신 방법은?
    
    process간의 통신은 IPC(inter process communication)이라고 불리우며
    
    kernel 이 직접 메세지를 전달하는데 참여하는 message passing, 프로세스간 IPC 의 목적을 위해 특별히 할당한 메모리를 공유하는 방식인 shared memory 이렇게 두가지 방식으로 소통한다.
    
    Thread 간의 통신은 thread의 특성상 같이 공유하는 메모리 공간을 이용하여 일어난다.
    
    구체적으로는 data section 에서 구현된 message queue 를 이용하여 통신을 하게된다.
    
- process 와 program 의 차이는?
    
    program은 파일 시스템에 존재하는 실행파일
    
    process는 program 을 실행시켰을때 하나의 인스턴스
    
- process 에는 어떤 상태들이 있는가?
    
    ready, running, waiting, new, terminated
    
- 그럼 thread간의 context switching 은 어떻게 일어나는가? 다른가?
    
    state, id 등 다 있을텐데 어떤 점이 다른지, PCB 를 대체하는 그에 대응하는 다른 자료구조가 있는지 조사가 필요해보임
    
- 

# Process란?

## process의 정의

- program in execution : 실행된 프로그램 중 하나의 인스턴스
- 현대 컴퓨팅 시스템에서 작업의 단위

## process 의 메모리 구조

process는 프로그램이 실행되었을 때 (정확히 말하자면 CPU에 의해 execute 되는 instruction) 의 하나의 instance입니다. Process는 생성될 때 memory 에 특정 부분을 할당받습니다. 이렇게 생성된 프로세스는 할당받은 memory 영역을 4가지 파트로 나누어 관리합니다.

![Untitled](/public/img/OS/processthread/Untitled.png)

- text 영역
    
    수행해야될 코드(instruction) 들이 저장되어 있는 공간입니다.
    
- data 영역
    
    전역변수가 저장되어 있는 영역입니다. 
    
- heap 영역
    
    프로그램이 수행될 때 동적으로 생성된 데이터가 할당되는 부분입니다.
    
    사용자가 직접 관리할 수 있는 영역으로 런타임시에 크기가 결정됩니다. 
    
- stack 영역
    
    새로운 함수 호출이 일어날 때 데이터가 쌓이는 부분입니다. 함수 내의 지역 변수나 parameter, return address등이 저장되어 있는 공간입니다. 
    
    주소를 할당할 때 높은 주소부터 낮은 주소 방향으로 할당을 받습니다.
    
- heap 영역과 stack 영역의 주소할당 방향이 반대인 이유
    1. 효율적인 메모리 관리
    2. buffer overflow 로 인한 보안상의 문제 해결

### C 프로그램의 메모리 할당

![Untitled](/public/img/OS/processthread/Untitled%201.png)

## process 의 상태(state)

특정 process 의 상태는 program counter (PC) 과 다양한 register에 의해 나타납니다.

![Untitled](/public/img/OS/processthread/Untitled%202.png)

- new : process is being created
    
    process 가 만들어지면 메모리도 할당해야 하고 PCB 도 만들어야 하는 등 process 가 구색을 갖추려면 다양한 일들이 필요합니다. 그래서 process 가 생성되고 나서 바로 Ready 상태로 가는 것이 아니라 준비 시간을 가지고 ready 상태로 바뀝니다.
    
- runinng : Instructions are being executed
- waiting(blocked) : the process is waiting for some event to occur
- ready : the process is waiting to be assigned to a processor
- terminated : process has finished execution
    
    process 가 생성될때와 마찬가지로 process 가 없어질 때 해야하는 작업들이 여러가지 있기 때문에 해당 일을 하는데 따로 state 가 필요합니다.
    

### ready 와 waiting 의 차이점

ready 상태는 process 가 scheduler 에 의해 CPU 를 차지할 수 있는 상태일 때 ready 상태입니다. 만약 ready 상태에서 scheduler 가 해당 process 를 run 시켜주면 process 는 CPU 를 통해 자신의 instruction 들을 실행시킵니다.

그에 반해 waiting 상태(block 상태라고도 많이 함)의 process 는 다른 I/O event 를 기다리는 process 입니다. 

간단히 얘기해 보자면 현재 running 상태의 process 가 키보드의 입력을 받아야 된다고 했을 때 running 상태에 계속 있으면 CPU 를 계속 점령하고 있는 상태에서 컴퓨터가 멈춰버리는 상황이 올 것입니다.

그래서 process 의 waiting 상태를 하나 더 만들어서 **비동기적으로 IO event 를 처리**할 수 있도록 하여 효율적으로 CPU 를 사용할 수 있도록 하는 것입니다.

## PCB (process control block)

![Untitled](/public/img/OS/processthread/Untitled%203.png)

각 process 들은 운영체제로부터 process control block 에 의해 인식됩니다. 고로 PCB 는 process 마다 하나씩 존재하며, process 생성 시 만들어지고 main memory 에 유지됩니다.

여기에서 이 PCB 는 운영체제가 프로세스를 제어아기 위해 정보를 모아놓는 곳으로 process state, register, memory information, I/O information, process 의 cpu 이용량 등이 저장되어 있습니다.

이런 정보들을 가지고 운영체제는 각각의 process 들의 정보를 받아다가 context switching 이 원활히 일어날 수 있도록 합니다. 

### PCB 의 구성요소

- process ID
- Process State
- Program Counter
- Register Information
- Scheduling Information
- Memory related Information
- CPU 사용시간, 실제 사용된 시간
- 입출력 상태 정보

## IPC (inter-process communication)

현대의 컴퓨터는 한번에 다수의 process 가 실행이 됩니다. 이 process 들은 서로 독립된 메모리공간을 가지고 독립된 일을 하지만, 필요한 경우에는 process 간의 통신을 이용하여 작업을 하기도 합니다. 

이렇게 process 들끼리 서로 영향을 끼치는 process 를 cooperating process 라고 부릅니다.

이런 **process cooperation 을 해야하는 이유**는 여러가지 있습니다.

- Information Sharing
    
    copy & paste 기능 같이 다수의 application 들이 사용해야하는 데이터가 같은 데이터일 경우에 해당 데이터에 대해 access 를 할 수 있도록 해야하기 때문에
    
- Computation Speedup
    
    특정 작업을 할 대 더 빠르게 하기 위해 subtask 로 쪼개서 parallel 하게 실행시키는 경우
    
- Modularity
    
    어떤 시스템을 모듈 방식으로 개발한다하면 시스템의 기능을 여러개의 process 나 thread 로 나누게 되는데 이 때 process 들 끼리 협력하는 상황이 생기는 경우
    

그러나 기본적으로 한 process 는 다른 process 의 정보를 보안상의 문제 때문에 읽어올 수 없습니다. 그래서 IPC 에는 기본적인 두가지 모델을 제공합니다.

![Message Passing 방식](/public/img/OS/processthread/Untitled%204.png)

Message Passing 방식

![Shared Memory 방식](/public/img/OS/processthread/Untitled%205.png)

Shared Memory 방식

### message passing

OS 가 memory protection 을 위해 대리 전달해주는 것을 말합니다.

- direct communication
    
    process A 가 메세지를 OS 한테 주면 OS 가 process B 에게 해당 메세지를 직접 전달하는 형태
    
- indirect communication
    
    process A 가 메세지를 OS 한테 주면 process B 가 해당 메세지를 OS 에서 가져오는 형태
    

그러나 이런 message passing 방식은 ㅂ너거롭고 overhead 가 심하다는 단점이 있습니다.

### shared memory

두 process 간에 공유된 메모리를 생성 후 해당 메모리를 이용하여 통신하는 것을 말합니다. 

Message passing 방식과는 다르게 OS 를 직접적으로 참여시키지 않고 따로 통신용 메모리를 할당하여 해당 메모리를 쓰고 읽는 방식으로 소통합니다.

하지만 제대로 읽고 쓰기 위해서는 동기화를 제대로 해줘야 된다는 단점이 있습니다.

# Thread란?

## Thread 의 필요성

![출처 :[https://dariaz.com/2015/10/18/a-simple-chat-application-with-wcf/](https://dariaz.com/2015/10/18/a-simple-chat-application-with-wcf/)](/public/img/OS/processthread/Untitled%206.png)

출처 :[https://dariaz.com/2015/10/18/a-simple-chat-application-with-wcf/](https://dariaz.com/2015/10/18/a-simple-chat-application-with-wcf/)

채팅 프로그램으로 예를 들어봅시다. 

우리가 채팅 프로그램을 쓸 때 프로그램 내부에서 지속적으로 해줘야 하는 일은 크게 두가지로 나눠보자면

1. 우리가 키보드로 타이핑하는 문자열을 화면에 표시
2. 상대방이 보낸 메세지를 받아서 화면에 표시

이렇게 나눠볼 수 있습니다.

그러나 우리가 보통 코드를 짜서 실행시킬 때 위에서 아래로 순차적으로 실행이 되는데, 이 채팅 프로그램의 user 입력을 받는 코드가 (c++ 의 경우 cin, python 의 경우 input() 이겠죠?) 가 있을 겁니다. 

이 부분을 실행시킬 경우에는 다른 user 에게 메세지 패킷이 네트워크로 도착할 경우에도 컴퓨터는 저희의 input을 기다리고 있으므로 상대방의 메세지는 받을 수 없을 겁니다.

이 문제를 해결하기 위해 기존에는 복수의 process들을 통해 user 의 입력을 받는 동시에 다른 user의 메세지를 수신할 수 있도록 하였습니다. 이 2가지 기능 외에도 눈에 보이지 않는 수많은 기능들이 기저에서 수많은 프로세스로 동시에 처리가 되었을겁니다.

하지만 이렇게 같은 채팅 프로그램을 실행시키는 process 들을 많이 만들면 process마다 메모리공간을 할당해야하고, IPC 도 설정을 해줘야 하는 등 프로그램이 커지고 시간이 지날수록 비효율이 크다는 단점이 있었습니다.

그래서 이를 해결하기 위해 하나의 프로세스 안에 여러 흐름을 만들고, 이 흐름들은 같은 자원(메모리) 들을 공유하도록 하는 multi-thread 환경을 구축하여 하나의 thread 는 유저의 입력을 받고, 다른 thread는 다른 유저의 메세지를 수신하도록 하여 해결하였습니다. 

![Untitled](/public/img/OS/processthread/Untitled%207.png)

위의 그림과 같이 하나의 process안에서 나뉘는 thread는 process 가 할당받은 data, text, heap 영역을 공유받고 따로 stack 영역만 할당받아 메모리 공간을 적게 차지하고 각 흐름(thread) 들 끼리 공유하는 메모리공간으로 자유롭게 소통하는 등의 여러 이점이 있어 thread는 현대까지 유용하게 쓰이고 있는 개념으로 남았습니다.

## Thread의 정의

- 제어의 흐름을 의미하는 것으로 process 의 실행의 개념만을 분리한 개념
- process 의 구성을 크게 실행과 실행환경부분으로 나눌 때 thread 는 process 의 실행 부분을 담당함으로 **실행의 기본단위**로 정의

## Thread 의 장점

- Responsiveness : 응답성 좋은 application 을 구성할 수 있게 된다.
    
    하나의 어플리케이션 안에서 I/O 입력이나 다른 오래걸리는 일을 처리할 때 따로 처리하여 user 에게 멈춰있는 어플리케이션이 아닌 뒤에서 복잡한 일을 돌리고 그 외의 일들은 할 수 있게끔 한다.
    
- Resource Sharing : 메모리 자원을 공유한다
    
    원래 IPC 를 이용하여 process 들끼리 소통했으나 구성하는 것 자체가 소모적인 일이고 힘들다. 
    
    Thread 를 이용하면 이런 소통에 대한 걱정을 덜 수 있다.
    
- Economy : 메모리 효율이 좋다.
    
    Memory 할당을 stack 영역만 따로 해줘도 되어서 메모리 소모가 적다.
    
- Scalability : 확장성이 좋다.
    
    multi-processor 환경에서 병렬로 일을 수행할 수 있다. 
    

### 참고문헌

Operating System Concepts 10th

양햄찌님 블로그 : [https://jhnyang.tistory.com/368?category=815411](https://jhnyang.tistory.com/368?category=815411)
