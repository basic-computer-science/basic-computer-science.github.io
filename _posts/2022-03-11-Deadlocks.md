---
layout: post
title:  "Deadlock"
categories: OS
---


# Deadlocks

# Questions

- 
- DeadLock 에 대해 설명해보세요
    1. deadlock 의 정의
    2. 필요조건
    3. deadlock 의 처리방법
        1. prevention
        2. avoidance
        3. detect & recover
- bankers algorithm 문제 설명

# Deadlock 이란?

Multiprogramming 환경에서 다수의 thread 가 제한된 자원을 사이에 두고 경쟁하는 일은 흔한 일입니다. 이 상황에서 만약 thread 가 하나의 resource 를 요청하는데 그 resource 가 다른 thread 에 의해 사용이 되고 있으면 요청한 thread 는 waiting 상태에 들어가게 됩니다.

그러나 그 resource 는 다른 thread 에 의해 계속 이용이 되어 해당 자원을 **요청한 thread 는 영원히 자원할당을 받지 못하고 영원히 waiting 상태**에 있을 수 있습니다. 이러한 상황을 우리는 deadlock 상태라고 정의합니다.

> **thread 간에 서로가 상대방이 자원을 내놓기를 바라면서 무기한 연기 상황에 빠지는 것**
> 

![[https://wakestand.tistory.com/253](https://wakestand.tistory.com/253)](/public/img/OS/deadlocks/img1.daumcdn.png)

[https://wakestand.tistory.com/253](https://wakestand.tistory.com/253)

![llustration-of-the-situation-of-deadlock-red-cars-at-a-crossroads.png](/public/img/OS/deadlocks/llustration-of-the-situation-of-deadlock-red-cars-at-a-crossroads.png)

이 deadlock 을 설명하기 위해서 가장 많이 언급되는 예시는 도로에서 발생하는 교착상태입니다. 

![Untitled](/public/img/OS/deadlocks/Untitled.png)

앞에 다루었던 semaphore 을 사용할때를 예시를 들자면 

1. process 0 이 자원 A 를 가집니다.
2. context switching 이 일어나
3. process 1 이 자원 B 를 가집니다.
4. 또다시 context switching이 일어나 
5. process 0 이 자원 B 를 요청하지만 process 1 이 점유하고 있어 wait 상태에 들어갑니다.
6. process 1 이 자원 A 를 요청하지만 process 0 이 점유하고 있어 wait 상태에 들어갑니다.

이런 식으로 진행이 될 수 있는데 만약 이런 context switch 순서로 프로세스들이 진행된다면 이제 process0, 1 은 더 이상 진행될 수 없는 **deadlock 상태에 빠졌다**고 합니다.

## Deadlock의 필요조건 (Necessary Conditions)

Deadlock 이 발생하기 위해서는 아래 4가지 조건이 모두 만족되어야 발생할 수 있습니다. 바꿔 말해서 아래 4가지 조건이 다 맞았다고 해서 꼭 deadlock 이 일어난다는 것은 아닙니다.

### Mutual Exclusion (상호배제)

한 thread 가 자원을 차지하고 있을 때 다른 thread 가 해당 자원을 원할 때는 기다려야 한다는 조건입니다. 

쉽게 말해서 **모든 자원들 중 적어도 한 자원은 하나의 thread 만 차지할 수 있다** 라는 조건입니다.

### Hold and Wait (점유와 대기)

**한 thread 가 이미 어떤 자원을 차지**하고 있을 때 **다른 종류의 자원을 추가적으로 요청**하는 상황이어야 합니다.

### No preemption (비선점)

자원들은 강제로 포기되는 것이 아닌 **자원을 차지하고 있는 thread 에 의해서만 free** 될 수 있습니다.

### Circular Wait (환형대기)

각 thread 가 요청하는 **자원을 차지한 thread 들 끼리는 환형 형태**가 되어야 합니다. 

# Deadlock 처리방법

## Deadlock prevention

Deadlock 이 발생할 수 있는 필요조건들 중 하나를 부정함으로 교착상태가 발생하지 않도록 미리 예방하는 방법입니다. 위의 4가지 조건 중 하나라도 충족이 안되면 deadlock 은 발생하지 않기 때문에 유효한 방법입니다.

### Mutual Exclusion

Sharable resource 들만 thread 안에서 다룬다면( 예를들어 read-only file )  mutual exclusion 의 상황을 없앨 수 있습니다.

하지만 현실적으로 non-sharable resource 가 존재할 수 밖에 없기 때문에 해당 조건은 배제하기가 쉽지 않습니다.

### Hold and Wait

hold and wait 상황이 일어나지 않도록 하기 위해서 시스템에서 

1. 각 thread 마다 사용하는 모든 resource 를 한꺼번에 할당하는 방법
2. 각 thread 가 resource 를 요청할 때 가지고 있는 다른  resource 를 가지고 있지 않음을 확인하여 주는 방법이 있습니다

하지만 이런 방식들은 자원이 쓰이지 않는 시간이 길어 자원 효율이 낮고 starvation 이 가능하다는 단점이 있습니다.

### No Preemption

하나의 thread 가 resource 들을 가지고 있는 상황에서 다른 resource 를 요청할 때 

1. 요청한 thread 의 원래 가지고 있던 모든 resource 를 강제적으로 포기하도록 하는 방법이 있습니다.
2. 그 thread 가 요청한 resource 를 가지고 있는 thread 에게 해당 자원을 포기하게 하고 요청하는 thread 에게 가져다 줍니다

그러나 이런 방식은 lock 이나 semaphore 같은 상황에서는 일반적으로 사용할 수 없다는 단점을 가지고 있습니다.

### Circular Wait

위의 3가지 방식들은 실질적으로 사용이 불가능한 방법들입니다.

하지만 이 circular wait 을 일어나지 않게 하는 방법은 실용적으로 쓰일 수 있는 방법입니다.

모든 resource 에 번호를 부여하여 프로세스가 자원을 요구할 때 오름차순으로만 요구를 할 수 있도록 합니다. 예를 들어 한 thread 가 2번의 자원을 가지고 있으면 1번 자원의 요구는 불가하게, 3번은 가능하게 하는 것입니다.

## Deadlock Avoidance

Deadlock prevention 은 자원의 활용도가 떨어지는 단점을 가지고 있어 이를 보완하기 위한 방법으로 **추가적인 사전 정보**를 통해 **deadlock 이 될 수 있는 자원 할당을 허락하지 않는 방법**인 deadlock avoid 방법입니다.

### Safe State

시스템이 deadlock 을 일으키지 않으면서 각 thread 가 요청한 최대 요구량만큼 필요한 자원을 할당해 줄 수 있는 상태로 **안전순서열이 존재하는 상태**를 의미합니다.

만약 이런 **안전순서열이 하나도 존재하지 않으면 unsafe state** 이라고 하고 이는 deadlock 의 필요 조건입니다.

### Resource Allocation Graph Algorithm (RAG algorithm)

### Banker’s Algorithm

Edsger Dijkstra 가 개발한 알고리즘으로 교착 상태에 빠질 가능성이 있는지 를 판별하는 알고리즘 입니다. 

즉 은행원 알고리즘에서 운영체제는 **safe state 를 유지할 수 있는 요구만을 수락**하고 **unsafe state 를 초래할 수 있는 사용자의 요구는 나중에 만족될 수 있을 때까지 계속 거절**하는 방식으로 avoid 합니다.

이 은행원 알고리즘이 수행되기 위해 시스템은 아래 3가지의 정보가 필요합니다.

1. 각 고객들이 얼마나 맥시멈으로 돈을 요구할지 (**Max**)
2. 각 고객들이 현재 빌린 돈이 얼마인지 (**Allocated**)
3. 은행이 보유한 돈이 얼마인지, 빌려줄 수 있는 돈이 얼마인지 (**Available**)

|  | Allocation | Max |
| --- | --- | --- |
|  | A         B         C | A        B         C |
| T0 | 0         1          0  | 7         5         3  |
| T1 | 2         0          0  | 3         2         2 |
| T2 | 3         0          2  | 9         0         2 |
| T3 | 2         1          1 | 2         2         2 |
| T4 | 0         0          2  | 4         3         3 |

|  | A | B | C |
| --- | --- | --- | --- |
| Available | 3 | 3 | 2 |

|  | Need |
| --- | --- |
|  | A         B         C |
| T0 | 7         4          3  |
| T1 | 1         2          2  |
| T2 | 6         0          0  |
| T3 | 0         1          1 |
| T4 | 4         3          1  |

## Deadlock Detection

Deadlock 발생을 허용하고 발생 시 감지하는 것으로 deadlock 이 발생한 것에 대해서 회복을 시키는 방법입니다.

Deadlock detection algorithm 에서 사용하는 사전 정보는 allocation(할당된 자원 수), available(할당 가능한 자원 수), request(요구하는 자원 수) 가 있습니다.

Bankers 알고리즘과 비슷한 정보들을 요구하지만 Max 값을 받지 않는 이유는 이 경우엔 deadlock 이 현재 상황에서 발생했는지 알아내는 것이 목표이기 때문입니다.

![Untitled](/public/img/OS/deadlocks/Untitled%201.png)

## Recovery from Deadlock

교착상태 발견 후 circular wait 을 배제시키거나 자원을 중단하는 메모리 할당 기법입니다.

### Process & Thread Termination

간단히, deadlock 이 발생함을 감지하고 해당 thread 를 죽이는 방법입니다.

### Resource Preemption

특정 resource 를 가지고 있는 process 들의 resource 를 다른 process 에게 강제적으로 부여하는 방법입니다.

### 참조

Operating System Concepts 10th edition

Kseo 님 블로그 : [https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=klp0712&logNo=220882367147](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=klp0712&logNo=220882367147)

양햄찌님 블로그 : [https://jhnyang.tistory.com/notice/31](https://jhnyang.tistory.com/notice/31)
