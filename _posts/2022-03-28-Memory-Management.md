---
layout: post
title:  "Memory Management"
categories: OS
---

# Memory Management

### 들어가기 전에

컴퓨터 시스템의 목표는 프로그램을 실행시키는데 있습니다. 이 중 프로그램과 그 프로그램이 접근하는 데이터는 실행 도중에는 **메인 메모리**에 적어도 어느 한 부분 정도는 적재되어야 합니다.

여태 운영체제 정리에서는 OS 가 process 들을 어떻게 효율적으로 처리할지 알아봤었습니다. 

지금부터는 이 process 들을 담고 있는 memory, 즉 메인 메모리에 대해 알아볼 것입니다.

# Background

## 하드웨어 기본 구성

**CPU 가 직접 접근할 수 있는 저장소**는 메인메모리와 레지스터 밖에 없습니다. 어셈블리 명령어에서도 데이터 접근을 메모리 주소값으로 하지 디스크 주소값으로는 하지는 않습니다. 그래서 어떤 한 instruction 이 실행이 될 때, 그 명령줄에 의해 사용되는 데이터는 CPU 가 직접 접근할 수 있는 **direct-access memory** 에 있어야 합니다. 만약 없다면 CPU 가 사용할 수 있도록 메모리에 옮겨야 겠죠.

이렇게 메모리에 올라간 프로세스들을 효율적으로, 안전하게 처리하기 위해 메모리 구조는 발전해 왔습니다.

### 메모리 접근 성능

보통 CPU 가 레지스터에 접근할 때는 한 CPU cycle 내로 접근이 가능한데 비해, CPU 가 메인 메모리에 접근하려고 하면 많은 CPU cycle 을 필요로 합니다. 이런 상황에서는 processor 가 요구하는 데이터가 없기 때문에 stall 하게 됩니다. 

하지만 이렇게 메모리에 접근할 때마다 stall 하는 것은 메모리에 접근하는 명령이 흔하기도 하고, 매우 비효율을 초래합니다. 이를 해결하기 위해서 CPU 가 빠르게 접근할 수 있는 **cache** 를 사용하여 성능을 높일 수 있습니다.

### 메모리 접근 security

성능도 중요하지만 instruction 에 따라 올바른 메모리주소로 매핑되고, 다른 메모리에 침범하지 않게 보호하는 일도 중요합니다. 

![Untitled](/public/img/OS/memory/Untitled.png)

각 프로세스가 분리된 메모리 공간에 있는걸 보장하기 위해서 base register, limit register 이 두개의 레지스터를 이용합니다. 

**Base register 는 프로세스의 가장 낮은 주소값**을 담고 있고, **limit register 는 그 프로세스가 가지는 용량**을 담습니다. Process 는 이 두 register 을 가지고 CPU 에게 접근할 수 있는 주소값을 제한할 수 있습니다.  user mode 로 돌아가는 프로그램이 다른 메모리영역이나 OS 영역에 접근하려고 하면 fatal error 을 발생하여 security 를 보장해줍니다.

![Untitled](/public/img/OS/memory/Untitled%201.png)

## Address Binding

Address 는 유저 프로그램이 얼마나 실행됐는지에 따라 다양한 형태로 존재하게 됩니다. 프로그램의 source program 에서는 주소값이 하나의 변수명으로 symbolic 하게 주어질 것입니다. 컴파일러는 보통 이 symbolic 한 주소값을 relocatable address 로 묶고, linker 가 이 relocatable address 를 absolute address 로 또 묶어줍니다. 이런 binding 들은 한 address space 에서 다른 address space 로 바꿔주는 역할을 합니다. (linking & compile 과정 설명: [https://jhnyang.tistory.com/40](https://jhnyang.tistory.com/40) )

일반적으로, instruction, 데이터 들을 메모리 주소에 binding 하는 일은 여러 단계로 걸쳐서 진행이 됩니다.

![Untitled](/public/img/OS/memory/Untitled%202.png)

- **코드를 컴파일 할 때** 프로세스가 메모리의 어떤 부분에 적재될지 **명시가 되어있으면** 컴파일 타임에서 코드들이 해당 주소값부터 담아질 것입니다. 만약 starting address 를 바꿔줘야 한다면 컴파일을 다시해야합니다.
- 만약 **compile time 에 프로세스가 어디 있을지 정해지지 않으면** 컴파일러는 relocatable code 를 생성하여 **load time** 에 어디 있을지가 정해집니다. 만약 starting address 를 바꾸줘야 한다면 유저코드를 reload 하면 됩니다.
- 만약 프로세스 진행중에 다른 메모리 부분으로 옮겨질 수 있다면, **run time** 까지 binding 이 연기되어야 합니다. 거의 모든 OS 가 이 방법으로 프로세스들을 bind 시킵니다.

## Logical vs Physical Address space

**CPU 에 의해 참조되는 주소는 logical address**, **실제 메모리**를 참조하는 memory address register 가 참조하는 주소는 **physical address** 라고 불립니다.

만약 address binding 이 compile time, load time 에 이뤄진다면 논리적 주소와 실제 주소는 같게 되지만, run-time 에 binding 이 이뤄진다면 달라지게 됩니다.

![Untitled](/public/img/OS/memory/Untitled%203.png)

이런 run-time mapping 은 **memory management unit (MMU) 에 의해 이뤄지게 됩니다.** 

MMU 는 여러가지 policy 에 의해 mapping 방식이 달라지게 되지만, 간단히 예를 들면 아래와 같이 작동합니다.

![Untitled](Memory%20Man%20ebe93/Untitled%204.png)

CPU 에서 참조할 때는 346 의 주소값에 있는 데이터를 참조했지만 MMU 에서 base register:14000 값을 더해줘서 실제 주소값 14346 에 있는 데이터를 참조하여 명령을 수행한다는 간단한 예시였습니다.

## Dynamic Linking (Shared Libraries)

**Dynamic lilnked libraries (DLLs) 은 유저 프로그램이 실행될 때 링크되는 시스템 라이브러리**를 칭합니다. 

OS 중 static linking 만 지원하는 시스템은 시스템 라이브러리들이 유저 프로그램이 컴파일 될 때 같이 컴파일 되어 분리되지 않고 하나의 프로그램처럼 보이게 됩니다. 이렇게 다이나믹 링크를 사용하지 않으면 프로그램의 용량을 증가시킬  뿐 아니라 메모리를 많이 차지하게 되기도 합니다. 

DLL 을 사용하면 메인메모리의 **하나의 DLL 객체로 다수의 process 가 접근하여 사용**할 수 있다는 점도 큰 장점입니다. 이래서 DLL 은 shared library 라고 불리우기도 합니다.

이렇게 동적으로 링킹을 하면 **버전 업데이트에도 자유롭다는 장점**이 있습니다. 만약 static한 방식으로 이루어진다면, 그 static library 를 참조하는 모든 프로그램들의 코드를 고쳐야 되는 반면에, dynamic library 는 자동으로 업데이트 된 내용들을 불러다 써서 유저 프로그램 단에서 별도의 작업이 필요가 없습니다.

실행되고 있는 프로그램이 dynamic library 에 있는 routine 을 참조하면, DLL 이 저장되어 있는 메모리의 영역으로 주소를 이동하여 참조가 일어나게 됩니다. 

## Dynamic Loading

여태까지 우리는 프로그램을 실행시킬 때 프로그램 전체를 메모리에 올려서 작업한다는 가정하에 메모리를 다뤘었습니다. 하지만 프로그램이 비대해서 메모리보다 큰 경우에는 아예 올리지 못하여 실행시키지 못하는 오류가 있기 때문에, 이를 해결하기 위해서 dynamic loading 을 이용하여 **프로그램의 일부분만 메모리에 올려놓더라도 돌아갈 수 있게**끔 합니다.

Dynamic Loading 을 이용하면 모든 routine 들은 불리지 않는 이상 메모리에 로드되지 않다가 routine 이 다른 routine 을 부를때, relocatable linking loader 가 해당 routine 을 메모리에 올려주고 그에 따라 프로그램의 address table 을 업데이트 시켜줍니다. 

# 연속적인 공간에 메모리 할당

앞서 살펴본 그림과 같이 메인 메모리는 OS 와 다수의 process 들을 담고 있어야 합니다. 이 과정에서 메인 메모리는 최대한 효율적으로 데이터들을 메모리에 담아야 합니다. 

메모리는 보통 OS파트와 user process 파트로 나뉘는데 보통의 운영체제에서는 OS 파트를 높은 주소값에 저장하는 경향이 있어 이 방식으로 메모리를 어떻게 효율적으로 사용할 지 살펴보겠습니다.

## 메모리 보호

![Untitled](/public/img/OS/memory/Untitled%205.png)

전에 살펴보았던 base address & limit register 를 이용한 메모리 접근 security 방식과 거의 비슷합니다. 그나마의 차이점은 여기서의 address 는 CPU 가 참조하는 logical address 에서 에러검출을 한 다음 physical address 로 바꿔 메모리에 접근한다는 점입니다. 

## 메모리 할당

### variable-partition scheme

메모리에 할당하는데 있어 가장 단순한 방법으로 다양한 크기를 가지는 process 를 그대로 메모리에 넣어 시스템이 메모리의 어떤 부분이 비었고 어떤 부분이 사용되고 있는지를 파악하는 방법입니다.

![Untitled](/public/img/OS/memory/Untitled%206.png)

위의 그림처럼 process 가 메모리에 들어왔다가, 다 사용되면 나가면서 hole 을 발생시키게 됩니다. 그리고 나서 다른 process 가 들어온다면, hole 중에 적합한 빈 메모리에 할당을 해주게 될 겁니다. 

이때 만약 또다른 process 가 들어와 메모리 할당을 요청했지만, 적당한 hole 이 없다면 어떻게 될까요? 가장 간단한 방법은 그냥 reject 하여 에러를 일으키거나 wait queue 에 보관하여 나중에 다른 process 가 없어지면 들어가는 방식일 겁니다.

이렇게 빈 공간에 process 를 넣어줄 때 어떻게 넣어줄지 다양한 policy 들이 있을 겁니다.

- First Fit
    
    넣어줄 수 있는 빈 공간 중 첫번째
    
- Best Fit
    
    넣어줄 수 있는 빈 공간 중 가장 작은 공간
    
- Worst Fit
    
    가장 큰 공간에 넣어줌
    

이 중에서 storage 효율은 first fit 과 best fit 방식이 가장 뛰어나지만, 그 중에서도 first fit 으로 하는 것이 더 빠르게 동작한다고 합니다.

## 단편화(Fragmentation)

하지만 이런 first fit, best fit 전략들도 external fragmentation 이 일어날 때 처리하기가 곤란해집니다. 프로세스들이 실행되고 free 될 때 수많은 작은 hole 들이 발생하게 되는데, 이 때 다른 process 를 넣어주려고 할 때 **전체적으로 빈 공간은 넣어주려고 하는 process 보다 크지만, 부분부분 비어있어 넣어주지 못할 때  external fragmentation 가 있다**고 정의합니다.

External fragmentation 의 한 해결책은 compaction 을 통해 free space 들을 하나의 큰 free space 로 만드느 방법입니다. 하지만 compaction 은 대체적으로 시간이나 다른 자원이 많이 든다는 단점이 있습니다.

또 다른 해결책은 하나의 process 가 꼭 연속된 메모리 공간을 차지하지 않아도 되도록 하는 방법입니다. 이 개념은 paging 에서도 쓰이는 개념입니다.

# Questions

- MMU 란?
    
    추후에 더 다룰거기 때문에 추가해놓을것
    
- Fragmentation 이란?
    - external fragmentation
        
        **전체적으로 빈 공간은 넣어주려고 하는 process 보다 크지만, 부분부분 비어있어 넣어주지 못할 때  external fragmentation 가 있다**고 정의
        
        해결책 1 : compaction
        
        해결책 2 : contiguous 한 성질을 버리는것(paging을 이용)
        
    - internal fragmentation
- DLL 이란?