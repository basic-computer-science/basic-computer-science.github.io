---
layout: post
title:  "[OS]Cache"
categories: OS
---


# Virtual Memory

# Virtual Memory

앞서 memory management 단원에서는 메모리 공간을 어떻게 효율적으로 사용하여 최대한 빈공간 없이 사용할 수 있을까 에 대해 알아보며 paging기법을 주로 다뤘습니다. 하지만 앞서 다뤘던 메모리 관리 기법들은 전부 한 프로세스에서 돌아가는 모든 코드나 데이터가 메인 메모리에 올라와있는 상태에서 이루어졌습니다.

Virtual memory 는 메모리 관리 기법 중 하나로 현재 돌아가는 프로그램이 전부 메인 메모리에 올라와있지는 않지만 사용자, 개발자 입장에서는 내부 상황을 정확히 모르고 그냥 프로그램이 메인 메모리에 올라와있는 것처럼 사용하게 하는 기법을 말합니다.

> **가상메모리란 프로세스가 메모리에 다 올라와 있지 않더라도 실행시킬 수 있게 만드는 전략입니다.**
> 

## 장점

- 프로그램은 더이상 메인메모리에 의해 크기가 제한되지 않는다
- 프로그램이 메인메모리를 더 적게 차지하기 때문에, 더 많은 프로그램들이 돌아갈 수 있다.
- 적은 I/O 요청으로 더 빠르게 돌아갈 수 있다
    
    프로그램 중 빈번히 사용되는 부분은 극히 적으므로, 자주 사용하지 않는 코드들 보단 자주 사용되는 코드들을 메모리에 올림으로 disk 에 오히려 적게 접근하게 됩니다.
    

# Demand Paging

![Untitled](/public/img/OS/virtual_memory/Untitled.png)

프로그램 실행 시작시에 프로그램 전체를 디스크에서 메인메모리에 올리는 대신 초기에 필요한 페이지만 로드하여 Virtual Memory 기법이 가능하게 합니다. 이처럼 현재 필요한 페이지만 메모리에 올리는 것을 Demand Paging(요구 페이징) 이라고 합니다. 

## Page Fault (페이지 부재)

페이지 부재는 위에서 살펴본 CPU 가 접근하려는 페이지가 메모리에 없는 경우(page table 의 valid bit = 0)입니다. 

아래는 demand paging 의 상황에서 만약 page fault 가 발생했을 때 시스템 내부적으로 일어나는 일입니다.

![Untitled](/public/img/OS/virtual_memory/Untitled%201.png)

1. 내부적인 테이블(pcb 안에 있는) 을 확인하여 해당 호출이 valid한 메모리 호출인지 아닌지를 판단
2. 만약 해당 호출이 invalid 하였으면, 프로세스를 종료시킨다. valid 하면 해당 페이지를 메인 메모리로 불러오기로 한다
3. free frame 을 찾는다.
4. 디스크에서 해당 페이지를 찾아 해당 frame 에 할당하도록 스케줄링한다
5. 만약 디스크에서 읽는 작업이 완료되면 해당 페이지가 메인 메모리에 있다고 internal table 과 page table을 수정해준다.
6. trap에 의해 interrupt 된 코드를 재실행시켜준다.

## Pure Demanding paging

프로세스가 최초로 실행될 때 어떤 페이지가 필요한지 알 수 없으므로, 아무 페이지를 메모리에 올리지 않고 시작하는 방법입니다. 그래서 **프로그램이 실행되자마자 page fault** 가 발생하고, 순수하게 필요한 페이지만 올리게 됩니다.

Pure demanding paging 의 장점은 메모리를 최대한 효율적으로 사용할 수 있다는 점에 있지만, prepaging 보다는 느리다는 특징을 가지고 있습니다.

## Prepaging

pure demanding paging 과 반대되는 개념으로 프로그램을 실행할 때 필요할 것이라 판단되는 페이지를 미리 올리는 방법입니다. 

Prepaging 은 page fault 가 일어날 확률이 적으므로 속도면에서 빠르지만, 미리 올라간 페이지를 사용하지 않는다면 메모리가 낭비된다는 단점을 가지고 있습니다.

## Swapping vs Demand paging

Paging 을 다루는 단원에서는 page 를 빼고 메모리에 올리는 작업을 swapping 이라고 했는데 demand paging 과는 어떤 점이 달라서 구분해 놨는지 헷갈릴 수 있습니다.

일단 공통점은 앞서 설명한 바와 같이 디스크에서 필요한 값들을 메모리에 옮긴다는 것인데, swapping 은 프로세스를 통째로 올리고 demand paging 은 프로세스가 또 나눠진 페이지 단위로 디스크에서 메모리로 page 를 올린다는 개념입니다.

즉, **디스크에서 메모리로 올리는 단위**가 다르다고 볼 수 있습니다.

# Page Replacement

page fault 가 일어날 때 메인 메모리가 꽉 차있으면 특정 frame 에 있는 page를 빼고 디스크에 있는 page 를 로드시킬 수 있어야 합니다. 그러면 메인 메모리에 있는 page 들 중 어떤 page 를 빼는 것이 좋을까요?

![78538479-066e2f80-782c-11ea-97e6-94ed4e8ee699.png](/public/img/OS/virtual_memory/78538479-066e2f80-782c-11ea-97e6-94ed4e8ee699.png)

위의 그림처럼 page replacement가 일어날 때 아래와 같은 과정을 거칩니다.

1. 디스크에서 원하는 page의 위치를 찾습니다.
2. free frame 을 찾습니다
    1. 만약 free frame이 있다면 해당 frame 을 사용
    2. 만약 없다면 victim frame 을 찾기 위해 page-replacement algorithm을 사용합니다
    3. 디스크에 victim frame 을 저장하고(page-out) 그에 따라 page table, frame table 을 수정합니다 
3. free 된 frame 에 요청한 page 를 읽어오고(page-in) 그에 따라 page table, frame table을 수정합니다.
4. page fault가 일어나기 직전상태의 process를 실행시킵니다.

### Victim Page

위에서 page-out 을 해줄 page를 고를 때 해당 page 가 수정되지 않으면 disk에 도로 써줄(write 작업) 필요가 없으므로, CPU 입장에서 수정되지 않은 페이지를 고르는 것이 효율적으로 보입니다.

그러면 해당 페이지가 수정되었는지 안되었는지를 판단할 수 있어야 하는데, 이를 위해 page table에 modified bit(dirty bit) 을 추가하여 이를 검사하게 됩니다. 해당 페이지가 수정되었다면 이 bit 을 1로 두고 안되었다면 0으로 둡니다. 이를 이용해서 victim page 는 최대한 수정되지 않은 페이지를 선택하게 되는 것입니다.

![78538693-5c42d780-782c-11ea-9f59-09c204d9de90.png](/public/img/OS/virtual_memory/78538693-5c42d780-782c-11ea-9f59-09c204d9de90.png)

위의 그림에서는 0, 2, 3 page 가 victim page가 될 확률이 높습니다.

그럼 위 3개중 어떤 걸 victim page 로 고르게 될까요? 가장 간단한 방법은 랜덤하게 선택하는 거겠지만, 어떻게 선택하느냐에 따라 시스템의 성능이 달라질 수 있으므로, 이를 선택하는 다양한 page replacement algorithm 이 제시가 되었습니다.

## FIFO Page Replacement

가장 간단한 알고리즘으로 가장 먼저 page-in한 페이지를 먼저 page-out 시키는 방식입니다. 이를 사용한 이유는 초기화 코드가 더 이상 사용되지 않을 것이라는 아이디어에서 시작했습니다.

![78565911-7d6ded00-7859-11ea-9118-aa6c7231d533.png](/public/img/OS/virtual_memory/78565911-7d6ded00-7859-11ea-9118-aa6c7231d533.png)

### Belady’s Anomaly

상식적으로 프레임 수가 증가하면(메모리가 커지면) page fault 의 수가 줄어드는 것이 정상적입니다. 

하지만 위의 FIFO 알고리즘을 사용했을 땐, 오히려 frame 의 크기가 증가해도 page fault 가 증가할 수가 있습니다.

![78566093-b73ef380-7859-11ea-8256-1b3cf6722f03.png](/public/img/OS/virtual_memory/78566093-b73ef380-7859-11ea-8256-1b3cf6722f03.png)

![78566141-c160f200-7859-11ea-9513-bfddb0216a14.png](/public/img/OS/virtual_memory/78566141-c160f200-7859-11ea-9513-bfddb0216a14.png)

사실 FIFO 뿐 아니라 다른 replacement algorithm 에서도 발생할 수 있는 문제로 이를 해결하기 위해 더 성능이 좋은 replacement algorithm 을 찾았습니다.

## Optimal Page Replacement

오랫동안 참조되지 않을 page를 replace하는 기법입니다. 이름에서도 알 수 있듯이 가장 효율적인 페이지 교체 알고리즘입니다.

여기서 가장 오랫동안 사용되지 않을 페이지를 계산하기 위해 현제 시점에서 그 이후에 최초로 나타나는 시점의 거리를 dist로  두고 이 값이 가장 큰 페이지를 victim page 로 선정하게 되는 방식입니다.

![78566299-fa996200-7859-11ea-80c4-57a831a371a8.png](/public/img/OS/virtual_memory/78566299-fa996200-7859-11ea-80c4-57a831a371a8.png)

하지만 이 방법은 미래에 어떤 page가 불리울지 미리 알아야 하기 때문으로 opt방식은 현실적으로 구현이 불가능합니다.

## Least Recently Used(LRU) Page Replacement

LRU 방식은 최근에 사용되지 않으면 나중에도 사용되지 않을 것이라는 가정하에 가장 오랫동안 사용하지 않은 page 를 victim page 로 선정하는 기법입니다.

![78567504-c1fa8800-785b-11ea-9427-f6f611c2998e.png](/public/img/OS/virtual_memory/78567504-c1fa8800-785b-11ea-9427-f6f611c2998e.png)

### LRU의 구현

- Counter
    
    page table의 entry 마다 해당 page를 언제 사용했는지 marking 하고, 특별한 register로 logical clock 역할을 하게하여 메모리를 참조할 때마다 1씩 증가하도록 합니다. 그리고 만약 해당 page 가 참조된다면, clock register 의 값을 page entry 에 복사시켜 넣습니다.
    
    만약 page replacement가 필요하다면 가장 작은 time value를 가진 entry를 page-out 시켜주면 됩니다. 
    
    추가적으로, 이를 구현할 때 context switching이 일어난다 하더라도 시간값은 유지되도록, clock 값이 overflow가 일어나지 않도록 특별히 유의해야 합니다.
    
- Stack
    
    page number 을 담는 stack으로 LRU replacement를 구현할 수 있습니다. 
    
    page 가 참조될 때마다 해당 page 를 스택에서 빼주고 top에 다시 넣어줍니다. 이런식으로 구현하면 stack 의 바닥에 있는 page 가 바로 LRU 로 victim page 로 선정됩니다.
    
    이 구현에서는 stack 의 중간에 있는 entry 가 제거되어야 하므로, double linked list 자료구조를 사용하면 유용합니다. Update할때마다의 시간은 좀 증가하겠지만, replace를 해야할 Page를 찾는 일은 tail pointer에 있는 값만 빼주면 되므로 간편합니다.
    
    ![Untitled](/public/img/OS/virtual_memory/Untitled%202.png)
    

## LRU Approximation algorithms

현대의 많은 컴퓨터는 LRU 기법을 이용하여 페이지를 교체하지만, 하드웨어의 서포트가 충분하지 않는 컴퓨팅 환경에서는 LRU 를 구현하는데 제한이 있을 수 있습니다. 이를 위해 대안적으로, 해당 page가 참조되면 marking 하는 reference bit 을 이용하여 페이지 교체 알고리즘을 수행합니다.

### Reference bit algorithm

각 page는 0이라는 하나의 reference bit 을 가지고 있어 만약 해당 page가 참조된다면 1 로 bit 을 바꿔줍니다. 그리고 page replacement가 일어난다면 bit 가 0 인 page 를 교체합니다.

timer을 두고 주기적으로 모든 page 의 bit 을 0으로 바꿉니다. 이렇게 하면 0인 page는 unique 하지 않을 가능성이 높으므로 이렇게 고른 page 들 중 FIFO 를 한다던지 하여 victim page 를 골라줍니다.

하지만 이 알고리즘을 사용한다면 timer의 주기에 따라 모든 page 의 bit이 0으로 바뀌고 replace 된 page 가 바로 불리우는 상황이 있을 겁니다. 이 문제를 해결하기 위해 second-chance algorithm을 사용할 수 있습니다.

### Second-chance algorithm (clock algorithm)

second-chance algorithm 의 기반 알고리즘은 FIFO 입니다. FIFO 에 의해 victim page가 선택된다면 해당 page 의 reference bit을 살펴봅니다. 만약 해당 값이 0이면 page replacement 를 수행하고, 1이면 해당 bit을 0으로 수정하고 FIFO 상 다음 page 를 살펴봅니다.

이 알고리즘을 구현하는 방법에는 circular queue 를 사용하는 방법이 있습니다. 해당 queue 의 pointer은 다음에 교체해야 할 page 를 가르킵니다. 만약 빈 frame이 필요해지면, pointer 는 reference bit 이 0인 page 가 나올 때 까지 진행하면서, 지나간 page 들의 reference bit 을 0으로 바꿔주면서 이동합니다. 만약 victim page 를 찾으면, 해당 페이지를 교체합니다.

이 방법을 이용한다면 만약 해당 reference bit 이 전부 1일때는 한바퀴를 통째로 돌고 FIFO 방식으로 돌기 때문에 비효율적일 수 있습니다.

### Enhanced Second-chance algorithm

위의 second change algorithm 처럼 reference bit 을 보고, 추가적으로 modified bit(dirty bit) 을 참고하여 교체될지 결정하는 알고리즙입니다. 이 두 bit 을 참조하면 page 들은 아래 4개의 case 를 가질 수 있습니다. 

| reference bit | modified bit | 설명 | 교체 순위 |
| --- | --- | --- | --- |
| 0 | 0 | 최근에 사용되지도 않고 변경되지도 않은 경우 | 1 |
| 0 | 1 | 최근에 사용되지 않고 변경만 된 경우 | 2 |
| 1 | 0 | 최근에 사용되었으나 변경되지 않은 경우 | 3 |
| 1 | 1 | 최근에 사영되었고 변경도 된 경우 | 4 |

## Counting-Based Page Replacement

### Least Frequently Used(LFU)

가장 많이 적게 사용된 page 를 교체해주는 알고리즘입니다.

하지만 맨 처음에 엄청 불리우고 그 이후로는 자주 쓰이진 않는 코드가 교체되지 않는 문제가 있습니다.

### Most Frequently Used(MFU)

가장 많이 불리운 page 를 교체하는 알고리즘입니다. 해당 알고리즘은 가장 적게 불리운 page는 가장 최근에 불리웠다는 가정으로 교체해주지 않는 특징을 가지고 있습니다.

## Page-Buffering Algorithms

앞서 설명드렸던 구체적인 page replacement algorithm외에 추가적으로 적용시키면 더 빨라지는 알고리즘들이 있습니다. 이 중 하나로 free frame 을 미리 생성하고 갖고 있다가 필요할 때 할당해줘 메모리 요청을 한 프로세스가 다시 instruction restart 을 최대한 빨리 할 수 있도록 해줍니다.

Page fault 를 빨리 처리해야 프로세스가 빨리 실행될 수 있기 때문에, 이런 바쁜 시간이 아닌 시간이 남을 때(CPU가 idle할 때) free frame 을 미리 확보해 놓아 page fault가 발생한다면 frame 을 비울 필요없이 바로 disk에서 페이지를 찾아 넣어주도록 합니다.

이 방법을 더 좋게 사용하는 법은 미리 page-out을 시킬 때, 해당 프레임의 페이지를 지워주는 것이 아닌 old page 로 남겨 만약 해당 페이지의 request가 일어날 때 다시 해당 페이지를 참조하는 식으로 성능을 높일 수 있습니다.

이와 비슷한 방식으로 dirty page 를 write back 할 때에도 paging device 가 idle할때 미리 write back 하는 방식으로 컴퓨터의 하드웨어를 효율적으로 사용할 수 있습니다.

# Allocation of Frames

앞에선 메모리가 꽉 찼을 때 프로세스가 page fault 를 낸 상황에서 어떻게 원하는 page 를 메모리에 로드할까에 대해 다뤘었습니다. 

이제는 멀티 프로세스 환경에서 어떻게 free frame 을 프로세스에 잘 할당할 것인가에 대해 다뤄보겠습니다.

### Minimum Number of Frames

기본적으로 프로세스마다 주어지는 frame 의 개수에는 최소값이 시스템적으로 정의되어 있습니다. 만약 한 프로세스당 주어지는 frame 이 매우 적다면 page fault 가 많이 일어나 프로세스의 진행이 느려지는 효과가 있기 때문입니다.

### Frame Allocation Algorithms

현재 m 개의 빈 frame 이 있다고 할 때 프로세스가 n 개 돌아간다고 할 때 이를 할당하는 방법은 equal allocation 과 proportional allocation 이 있습니다.

- equal allocation
    
    OS 에 메모리를 할당하고 남는 메모리 공간 100 frame 이 있다고 하고 5개의 프로세스가 실행된다고 가정하면, 각 프로세스에 20개의 프레임을 주는 방식입니다.
    
- proportional allocation
    
    각 프로세스가 요구사항에 비례하여 프레임이 할당됩니다.
    

### Global vs Local Allocation

- local allocation
    - page replacement 가 일어날 때 메모리를 참조한 프로세스의 페이지만 replace 할 수 있는 방식입니다.
    - 프로세스에 할당된 페이지의 개수는 변하지 않아 프로세스 동작이 항상 비슷한 양상으로 흘러갑니다.
    - 그러나 잘 안쓰이는 프로세스의 페이지가 계속 메모리에 남아 메모리 낭비가 된다는게 단점입니다.
- global allocation
    - page replacement 가 일어날 때 프로세스 입장에서 내 page 인지 다른 process 의 page 인지 상관없이 replace 시킬 수 있는 방식입니다.
    - 우선순위가 낮은 프로세스의 페이지를 교체시켜 page fault 수를 줄여 성능을 높여줍니다.
    - 메인메모리 안의 페이지는 해당 프로세스에 연관되어 있을 뿐 아니라 다른 프로세스의 페이징 behavior 에도 연관이 있어서 같은 프로세스라도 외부상황에 따라 다르게 동작할 수 있습니다.

고로 보통 global replacement가 througput 이 좋게 나오는 경우가 일반적이어서 global replacement가 보통 쓰인다고 합니다.

### Global Page replacement algorithm 구현

특정 threshold 를 설정하여 이 기준보다 낮은 free frame 이 있으면 page replacement 를 하도록 하여 free frame list에서 모든 메모리 요청을 만족한다. 이 전략은 항상 어느 정도의 free memory가 존재하도록 보장해줍니다.

Threshold보다 적은 free frame 수가 되면 kernel routine , reaper 가 동작하면서 maximum threshold 까지 빈 프레임을 증가시킵니다. 

이 reaper 은 모든 replacement algorithm 을 적용시킬 수 있지만, 특히 LRU approximation을 사용합니다. 그래서 상황에 따라, 빈 공간이 적으면 LRU를 포기하고 FIFO 를 하거나 LINUX 같은 경우에는 OOM killer가 발동하여 OOM score 에 따라 점수를 매겨 프로세스들을 종료시켜 메모리를 확보한다고 합니다.

![Untitled](/public/img/OS/virtual_memory/Untitled%203.png)

### Non-Uniform Memory Access (NUMA)

시스템 내에 CPU 와 메모리가 어떻게 연결되었는지에 따라 CPU 마다의 메모리 접근속도가 다름에 따라 process 별로 latency group 으로 나누어 실행할 CPU 를 정해주는  architecture.

# Thrashing

일반적으로 메모리에 올라가는 프로세스의 개수가 증가할 수록 CPU 사용률은 올라가는게 당연하지만, 동시에 올라가는 메모리 개수가 늘어나면 오히려 CPU 이용율이 감소하는 현상이 일어납니다.

![Untitled](/public/img/OS/virtual_memory/Untitled%204.png)

이 현상이 발생하는 이유는 프로세스가 증가할수록 메인메모리에 비어있는 frame 개수가 줄어들게 되고 결국 모든 frame 이 차게 되서 page fault 가 많이 일어나 실제로 instruction 을 실행하는 시간보다 page-in/out 하는 시간이 커져 CPU 가 일을 오히려 못하게 됩니다.

> **Page Fault 가 너무 자주 발생하여 시스템이 돌아가는데 시간을 많이 잡아먹는 상황**
> 

## 해결방법

### 1. Global Replacement 보다는 Local Replacement 를 사용하는것

- 앞서 살펴 보았듯이 메모리 사용 효율이 떨어진다는 단점이 존재.

### 2. 프로세스당 충분한 수의 frame 을 할당한다.

얼만큼이 적당한가?

앞서 설명했던대로 frame 들을 할당하는 방법에는 2가지 방법이 있습니다.

- Equal Allocation
- Proportional Allocation

정적으로 프레임을 할당한다면 프로세스 크기를 고려하지 않으므로 매우 비효율적이어서 동적할당의 방법을 사용하는 것이 좋습니다.

### Working Set Model

 프로세스가 실행 중일 때 어느 페이지를 사용하는지 실험한 결과에서 Locality 성질이 성립한다는 것을 발견할 수 있었습니다. 

이를 이용하여 해당 프로세스가 실행되는 특정 시간에 따라 사용되는 페이지의 개수만큼 프레임을 할당시켜준다는 개념입니다.

![78572017-e48f9f80-7861-11ea-8ef0-df1a62f20411.png](/public/img/OS/virtual_memory/78572017-e48f9f80-7861-11ea-8ef0-df1a62f20411.png)

위 그림은 working-set model 의 동작을 나타낸 그림입니다. 변수 $\Delta$ 는 현재 시간 t 로부터 이전 값을 보는, OS 가 정해주는 값으로 만약 현재 $t_1$ 시간이라면, working set 은 과거에 불렸던 page 들로 {1, 2, 5, 6, 7} 이 되어 frame 을 5만큼 할당시키게 됩니다.

이렇게 각 프로세스마다 working set 의 크기를 다 합한 값을 D 라고 하면, 이 D 는 결국 각 프로세스가 필요로 하는 frame 수이므로 이 D 가 메인메모리의 총 frame 개수보다 적게 OS 가 관리하여 thrashing 이 일어나지 않도록 합니다.

### Page Fault Frequency

Thrashing 을 어떻게 방지할 것인가에 따른 해결책으로 page-fault frequency 의 방법이 제시됐습니다. 

만약 page-fault rate 이 너무 크다면 해당 process 는 더 많은 frame 을 할당해야 하고, 너무 낮으면 프로세스는 너무 많은 frame 을 가지고 있다는 의미가 됩니다. 

운영체제 입장에서는 프로세스의 frame 마다 page-fault 의 발생 빈도를 측정하여 아래와 같은 그래프로 나타낼 수 있습니다.

![Untitled](/public/img/OS/virtual_memory/Untitled%205.png)

그래서 OS 가 임의적으로 upper bound, lower bound 를 설정하여 page-fault 의 빈도에 따라 해당 프로세스에 frame 을 더 할당할 것인가, 덜 할당할 것인가를 판단하여 thrasing 을 해결하는 방법을 PFF 이라고 합니다.

## Page Size

현재 컴퓨팅 환경에서 페이지는 일반적으로  4KB ~ 4MB 의 크기를 가집니다. 이 페이지 크기에 따라 컴퓨터의 성능이 바뀌기도 하는데 어떤 요인으로 인해 바뀌는지 살펴봅시다.

- Internal Fragmentation
    
    페이지 크기가 클수록 페이지 안에 빈 공간이 발생할 확률이 높음
    
- Page-in/out 시간
    
    페이지가 클수록 읽어오는 데이터가 많아져 시간이 오래 걸리게 된다.
    
- 페이지 테이블 크기
    
    페이지가 클수록 페이지 개수가 줄어들어 테이블 크기도 줄어든다.
    
- Memory Resolution (해당 메모리에 필요한 데이터가 있을 확률)
    
    페이지 크기가 클수록 필요없는 데이터가 있을 확률이 높으므로 resolution 이 낮다.
    
- page-fault 발생 확률
    
    locality 와 관련하여 프로세스가 필요한 메모리가 페이지가 클수록 해당 페이지가 갖고 있을 확률이 높으므로 page-fault 가 적게 일어난다.
    

| page size big | page size small |
| --- | --- |
| Page fault 발생 확률 Down | Internal Fragmentation Down |
| page table 크기 Down | Page-in/out 시간 Down |
|  | Memory Resolution UP |

# Questions

- Virtual Memory란?
    - 정의 : 프로세스의 일부를 메모리에 올려 실행이 가능하도록 하는 기법
    - 장점, 쓰는 이유
        
        → (프로세스를 통째로 올리는거에 비해 뭐가 더 좋은가?)
        
        1. 프로그램의 크기를 메인메모리의 크기와 상관없이 크게 만들 수 있다.
        2. 프로그램이 메인메모리를 더 적게 차지하기 때문에, 더 많은 프로그램들이 돌아갈 수 있다.
        3. 적은 I/O 요청으로 더 빠르게 돌아갈 수 있다
    - 구현 : Demand paging 을 이용하여 page-in/out을 수행한다.
    - 
- Page Fault란?
    - 정의 : CPU가 요청한 page 가 없어 secondary storage 에서 페이지를 가져와야 하는 경우
    - 남는 frame이 없을 경우 victim page를 고르는 page replacement algorithm이 적용되어야함
    - page replacement algorithm 으로는 LRU 가 가장 많이 쓰이고 LRU 를 사용하지 못하는 컴퓨팅 시스템에서는 LRU approximation algorithm 으로 reference bit algorithm, second chance algorithm 을 사용함
- Thrashing 이란
    - 메인메모리에 올라오는 프로세스의 수가 많아지면 CPU utilization 이 원래는 높아져야 하는데, 특정 프로세스 이상이 되면 오히려 줄어드는 현상
    - 해결방법 : 프로세스당 충분한 양의 frame을 보장해줄 것
        
        → 그럼 프로세스당 얼마만큼의 frame 을 보장해야하는가?
        
        - working-set model을 적용하여 프로세스당 필요한 프레임 계산 가능
        - page fault frequency을 모니터링하여 lower bound, upper bound 에 따른 frame 할당을 변화시키면서 thrashing을 조절할 수 있음

[](https://velog.io/@doyuni/2020-01-03-0001-%EC%9E%91%EC%84%B1%EB%90%A8-zpk4wvjl1v)

[[운영체제 정리] 16: 가상 메모리 (Virtual Memory) | 완숙의 에그머니🍳](https://wansook0316.github.io/cs/os/2020/04/06/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EC%A0%95%EB%A6%AC-16-%EA%B0%80%EC%83%81%EB%A9%94%EB%AA%A8%EB%A6%AC.html)