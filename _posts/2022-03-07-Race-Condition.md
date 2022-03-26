---
layout: post
title:  "Operating System 이란?"

categories: OS
---

# Race Condition

### 들어가기 전에

협력하는 process 는 서로 다른 process에 의해 영향을 주거나 받을 수 있는 process를 지칭합니다. 이는 앞서 설명했던 IPC 나 직접적으로 공유되는 메모리영역 등에 의해서 발생할 수 있습니다. 

하지만 이런 shared memory에 다른 process들이 동시에 접근하는것은 데이터가 일치하지 않는 문제를 발생시킬 수 있습니다. 이 포스팅에서는 같은 데이터에 접근하는 process들이 순서에 맞게 올바른 작업들을 진행할 수 있도록 하는 다양한 방법들에 대해서 다룰 것입니다.

# Race Condition

두 process가 같은 buffer 을 공유해서 서로 통신하는 상황을 가정해봅시다.

통신하는 과정에서 producer process 는 buffer 가 비어있으면 해당 메세지를 buffer에다가 넣어줍니다.

```c
while (true) {
		/* produce an item in next produced */
		while (count == BUFFER SIZE)
		; /* do nothing */

	buffer[in] = next produced;
	in = (in + 1) % BUFFER SIZE;
	count++;
}
```

받는 consumer process 는 buffer 에 받을 메세지가 있다면 가져옵니다.

```c
while(true){
	while (count == 0)
		; /*do nothing*/
	next_consumed = buffer[out];
	out = (out+1) % BUFFER_SIZE;
	count--;

	/* consume the item in next_consumed */
}
```

위의 명령들을 각 process들은 실행하여 공용으로 사용하는 변수 `count` 의 값을 변화시킵니다. 

이 때 만약 `count` 변수가 5이고 producer와 consumer가 동시에 `count++`, `count--` 을 실행시켰을 때 count는 5여야 되지만 실제로는 그러지 않을 가능성도 존재합니다. 

이 `count` 변수에 1을 더해주고 빼주는 instruction은 machine language로는 실제로 한 줄의 명령어가 아닌 아래의 명령어들로 분리가 됩니다.

```c
register1 = count
register1 = register1 + 1
count = register1
```

```c
register2 = count
register2 = register2 - 1
count = register2
```

위의 3줄씩의 기계어가 수행될 때 context switching이 일어날 경우 아래와 같은 상황이 발생할 수 있습니다.

> **machine language 진행 과정**
> 
> 1. producer    execute    register1 = count             {register1 = 5}
> 2. producer    execute    register1 = register1 + 1  {register1 = 6}
> 3. consumer   execute    register2 = count             {register2 = 5}
> 4. consumer   execute    register2 = register2 + 1  {register2 = 4}
> 5. producer    execute    count = register1              {count = 6}
> 6. consumer   execute    count = register2             {count = 4}

이런 순서로 진행이 되어 최종적으로 `count` 변수에는 4의 값이 들어가게 됩니다. 이 상황은 두 process가 동시에 변수 count 에 접근하여 발생한 문제입니다. 

이렇게 다수의 process 가 같은 data에 동시에 접근하여 실행될 때, 실행 결과가 process 의 수행 순서에 따라 변할 수 있는 상황을 race condition 이라고 합니다.

위에서 제시된 Race Condition 을 방지하기 위해서 운영체제는 `count` 변수에 접근할 수 있는 process 를 특정 상황에서 제한해야하고 이를 우리는 동기화한다, synchronization이 일어난다고 표현합니다.

## Critical Section

위의 코드에서 발생한 상황처럼 다수의 process 에 공유된 하나의 data 에 access, update하는 코드블럭을 critical section (임계영역) 이라고 부릅니다.

위의 상황이 벌어지지 않도록 synchronization을 수행하는 일은,  한번에 하나의 process만이 critical section에 들어갈 수 있도록 하는 일입니다. 이를 해결하기 위한 solution은 아래 3가지의 조건을 만족시켜야 합니다.

1. Mutual exclusion (상호배제)
    
    특정 process 가 critical section 의 명령어를 수행하고 있을 때, 다른 process들은 critical section 의 명렁어를 수행할 수 없음
    
2. Progress (진행)
    
    만약 모든 process 가 critical section 을 돌고 있지 않고 특정 process들이 critical section에 들어가려고 할 때 critical section에 진입할 process 를 고를 때 다른 코드를 실행하고 있지 않은 process 들 중 하나를 골라야 함
    
3. Bounded waiting (한계대기)
    
    한 process가 자신의 critical section에 진입하고자 요청을 한 이후부터 이 요청이 accept 될 때 까지 다른 process가 그들의 critical section 에 진입할 수 있는 회수가 제한되어야 함
    
    무한정 대기가 없어야 한다.
    

# Solution

## Peterson’s Solution

Peterson’s solution은 critical section 문제를 소프트웨어적으로 해결하는 알고리즘입니다.

사실 이 방법은 현대 컴퓨팅환경에서는 critical section 문제를 완벽하게 해결하지 못할 수 있지만, 다양한 컴퓨팅 문제에서 해결할 수 있는 유용한 개념을 담고 있으므로 소개합니다.

피터슨 솔루션은 2개의 process간의 상황을 가정하고 이루어지고, 2개의 공유되는 변수 `turn` 과 `flag[2]` 을 통해 critical section을 해결합니다.

여기에서 `turn` 변수는 어떤 process 가 critical section 에 들어갈 차례인지 알려주는 변수이고 `flag` 배열은 해당 process가 critical section에 들어갈 준비가 되었는지 나타내주는 boolean 배열입니다. 

아래는 process i 의 코드입니다.

```c
while(true) {
	flag[i] = true;
	turn = j;
	while (flag[j] && turn == j) ;
	
	/* critical section */
	
	flag[i] = false;

	/* remainder section */
}
```

1. process i 가 critical section 에 진입하기 위해서 일단 들어갈 준비가 되었다고 flag[i] = true 를 시킵니다.  `turn = j` 로 함으로 process j 가 critical section 에 들어가고 싶은지, 들어가고 싶어하면 먼저 실행하도록 합니다. 
2. j 가 critical section 안에 있는 동안, i는 while 문을 무제한으로 돌면서 자기의 turn  을 기다립니다.
3. j 가 turn 변수를 i 로 바꿔주거나, `flag[j] == false`이면 while 문이 종료되면서 자연스럽게 i가 critical section 에 들어갑니다.
4. i 도 critical section을 다 지나면, `flag[i] = false` 를 함으로 critical section에 들어가고 싶지 않다는 걸 표시해줍니다.

만약 두 process i, j가 동시에 critical section에 들어가고 싶어하면 거의 비슷하게 turn변수가 i 와 j 로 설정될 것입니다. 만약 process i 가 좀 더 빨랐으면 turn = j → turn = i  설정이 되고 i 가 critical section 을 유일하게 돌릴 수 있게 될 겁니다.

그렇다면 피터슨 솔루션은 critical section을 해결하기 위한 3가지 조건을 만족시켰는지 확인해보면,

1. mutual exclusion
    
    process가 공유된 자원에 접근하려고 할 때 다른 process가 기다리지 않고, 내 차례여야 돌아갈 수 있으므로 만족
    
2. progress
    
    ```c
    while (flag[j] && turn == j)
    ```
    
    내 순서가 아니더라도 (turn =j 여도) 다른 process가 안쓰면 사용 가능
    
3. bounded waiting
    
    while문을 계속 돌고있는 process i 는 process j 의 critical section이 끝난 다음엔 무조건 돌아가므로 무제한으로 기다리지 않을 것이다.
    

## Hardware Supported Synchronization

앞서서는 소프트웨어적 해결책인 피터슨 해결책에 대해서 다뤘었습니다. 

앞서 말했듯이 소프트웨어 기반 해결책은 현대 컴퓨팅모델에서 유효하지 않을 가능성도 있습니다. 그래서 이를 해결하기 위해 하드웨어의 도움을 받아 critical section 문제를 해결할 수 있습니다.

### Memory Barriers

OS 단에서 memory barrier 이라는 명령어를 제공해 해당 명령어를 실행시키면 메모리상의 update 된 모든 변경점들을 다른 process 들에게 적용이 되도록 합니다.

그러나 매우 low-level 한 작업이고 특별한 상황에서 kernel developer 만 쓰는 특별한 명령어로 일상적으로는 쓰이지 않습니다.

### Hardware Instructions

현대 컴퓨팅 환경에서 많은 컴퓨터 시스템들은 변수를 변경하거나 swap 할 때 atomic 하게 바꿀 수 있도록하는 특별한 하드웨어적 명령을 제공합니다. 이 명령이 실행이 된다면 다른 interrupt 는 끼지 못합니다. 

이 특수한 명령어를 이용하여 우리는 critical-section 문제를 비교적 쉽게 해결이 가능합니다. 이런 instruction 은 크게 test_and_set() , compare_and_swap() 등의 명령어가 존재합니다.

Hardware instruction 중 대표적인 test_and_set() 에 대해 알아봅시다.

```c
boolean test_and_set(boolean *target)
{
		boolean rv = *target;
		*target = true;

		return rv;
}
```

test_and_set 의 구현 사항입니다.

해당 함수는 하드웨어적으로 쪼개지지 않는 작업으로 이루어져 있어 test_and_set 이 수행되는 중간 다른 process 로 context switching 되는 일은 없습니다.

```c
do{
		while(test_and_set(&lock))
			; /*do nothing*/
		
		/* critical section */
		lock = false;

		/* remainder section */

} while(true)
```

그래서 위와 같은 코드를 한 process 에서 진행할 때, while 문에서 lock 이라는 전역 변수가 참인지 거짓인지 체크하는 과정에서 interrupt 가 발생하지 않도록 할 수 있는 것입니다.

이렇게 하드웨어적으로 구현된 atomic 한 instruction 을 이용하여 mutual exclusion 을 달성할 수 있습니다.

## Mutex Locks

위에서 제공된 하드웨어 기반 해결책은 application 개발자들은 보통 쓰기 힘들다는 (디자인에 따라 못 쓰게 되있을수도) 단점이 있습니다. 이를 해결하기 위해 OS 디자이너들은 higher-level 소프트웨어 tool 인 mutex lock 을 제공하였습니다.

```c
aquire() {
		while (!available)
			; /*busy wait*/
		available = false;
}

release() {
		available = true;
}
```

그래서 우리는 이제 mutex lock 을 이용하여 critical section 을 보호하고, race condition 을 방지할 수 있게 되었습니다. 이는 `acquire()` 함수로 lock 을 얻고, `release()` 함수로 lock 을 푸는 방식으로 이루어집니다.

```c
while(true) {
		acquire();

		/* critical section */

		release();

		/* remainder section */
}
```

위의 acquire 함수나 release 함수는 atomic 하게 이루어져 있어 다른 process 가 끼어들지 못하게 되어 race condition 을 예방할 수 있습니다.

하지만 이러한 구현은 busy waiting 한다는 단점이 존재하여 CPU resource 를 낭비한다는 단점이 존재합니다. 이렇게 하나의 process 가 lock 을 얻기 위해 CPU 를 ‘spin’ 시키기 때문에 mutex lock 을 spin-lock 이라고 부르기도 합니다. 

## Semaphore

Semaphore 은 하나의 integer 변수로 두 가지 atomic operation 인 `wait()` 과 `signal()` 로만 접근이 가능한 변수를 칭합니다.

Semaphore은 mutex lock과는 다르게 signaling 메커니즘으로 돌아가는 동기화 기법으로 lock 을 걸지 않은 process도 signal 을 보내 lock 을 해제할 수 있다는 점에서 차이가 있다.

Semaphore가 mutex lock과 또 다른 점은 boolean 형태의 lock 변수를 다루는 것이 아닌 avaiable한 resource 의 수를 담고 있는 정수형 변수를 다룬다는 점입니다. 이 정수형 변수는 resource 의 상태를 나타내는 간단한 카운터라고 생각할 수 있습니다. 

### Busy wait 을 이용한 방법

```c
P(S) {
     while S <=0; // 아무것도 하지 않음 (반복문)
     S--;
 }

 V(S) {
     S++;
 }
```

### Waiting Queue implementation

```c
typedef struct {
		int value;
		struct process *list;
} semaphore;
```

```c
wait(semaphore *S) {
		S -> value--;
		if(S->value < 0){
				add this process to S -> list;
				sleep();
		}
}
```

```c
signal(semaphore *S){
		S->value++;
		if(S->value <=0) {
					remove a process P from S->list;
					wakeup(P);
		}
}
```

critical section에 들어갈 수 없다면 마냥 기다리기 보다는 process 자기 자신을 suspend 시켜 waiting queue에 넣어주는 방식을 택하면 위의 busy waiting 을 안하고 구현할 수 있습니다.

위와 같은 식으로 구현해도 value를 더하고 빼주는 과정에서 race condition 을 방지해야 하기 때문에 busy waiting을 해야하지만 lock을 거는 instruction 이 줄어들어 busy waiting 할 확률을 많이 줄일 수 있습니다.

위의 구현대로 하면 semaphore 의 value 는 음수가 나올 수 있어 음수만큼 waiting queue에 process들이 들어가 signal 함수가 불릴 때마다 waiting queue 에서 해당 process가 critical section에 들어갈 수 있게 됩니다.

# Questions

- race condition은 무엇입니까?
- critical section 이란?
- critical section의 해결책은?
- 해당 해결책이 가져야 할 조건은?
- peterson’s solution 이 현대 컴퓨팅 환경에서 안돌아가는 이유,  돌아가게 하려면?
    
    [https://snowfleur.tistory.com/126](https://snowfleur.tistory.com/126)
    
- mutex lock 방식과 semaphore 방식의 차이는?

### 참조 문헌

Operating System Concepts

양햄찌님 블로그 : [https://jhnyang.tistory.com/category/별걸다하는 IT/운영체제 OS](https://jhnyang.tistory.com/category/%EB%B3%84%EA%B1%B8%EB%8B%A4%ED%95%98%EB%8A%94%20IT/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C%20OS)
