---
layout: post
title:  "큐(Queue)"
date:   2022-03-02 19:00:00 +0900
categories: DataStructure
---

<br/>

# Questions

---

- **Stack과 Queue의 차이점은 무엇인가요? (N사 전화면접)**
- **Priority Queue의 동작 원리가 어떻게 되나요? (N사 전화면접)**
    
    
- **큐 / 우선순위 큐의 개념에 대해 설명해보세요.**
- **큐 삽입(enqueue) / 삭제(dequeue) 함수를 배열 / 링크드리스트로 구현하라.**
- **스택 2개로 큐 1개를 구현한 MyQueue 클래스를 구현하라.**

<br/>

# 큐(Queue)의 개념

---

큐는 배열과 함께 가장 쉬운 자료 구조 중 하나이다. 큐는 컴퓨터에서 가장 핵심적인 소프트웨어 중 하나인 운영체제에서도 많이 쓰인다. 또, 인터넷에서 네트워크 기능에서도 많이 사용된다.

![Untitled](https://user-images.githubusercontent.com/100582309/157604545-ebcd75f8-e6dc-4710-9914-681df09bfcfe.png)


Queue의 사전적 의미는 1. (무엇을 기다리는 사람, 자동차 등의) 줄, 혹은 줄을 서서 기다리는 것을 의미한다. 즉, 가장 먼저 넣은 데이터를 가장 먼저 꺼낼 수 있는 선입선출 = FIFO (First-In, First-Out) 방식의 자료구조를 말한다.

큐의 삽입 연산은 한 쪽 끝(rear)에서, 삭제 연산은 반대쪽(front)에서 일어난다. 이때, 큐의 리어에서 이루어지는 삽입연산을 **인큐(enQueue),** 프론트에서 이루어지는 삭제연산을 **디큐(dnQueue)**라고 부른다.    ⇒ 접근방법은 가장 첫 원소와 끝 원소로만 가능

큐에 요소가 하나도 들어있지 않을 때는 **공백(empty)** 상태라 하고 꽉 차서 더 이상 요소를 집어넣지 못하 경우 **포화(full)** 상태라고 한다.

## 큐의 활용 예시

---

큐는 주로 데이터가 입력된 시간 순서대로 처리해야 할 필요가 있는 상황에 이용한다.

- 우선순위가 같은 작업 예약 (프린터의 인쇄 대기열)
- 프로세스 관리
- 너비 우선 탐색(BFS, Breadth-First Search) 구현
- 캐시(Cache) 구현

## 큐의 구현

---

Python으로 큐를 구현하는 방법은 세 가지 정도가 있으며 각 방법마다 장단점이 있다.

1. List 자료형 사용
2. Collections 모듈의 deque 사용
3. queue 모듈의 Queue 클래스 사용

<br/>

### 1. List 자료형 사용

- **구현**
    
    파이썬의 기본 자료형인 list를 사용한 방법이다. `.append(x)` 메소드를 사용하여 배열의 끝에 데이터를 추가하고, `.pop(0)` 메소드를 사용하여 첫 데이터를 제거한다. 스택을 구현할 때와 반대로 메소드를 사용하면 큐 자료구조를 사용하는 효과가 난다. `insert(0, x)` 함수를 사용하면 맨 앞에 데이터를 넣어 반대 방향으로 삽입도 가능하다.
    
    ```python
    # 큐의 맨 끝에 데이터 삽입
    queue = [1, 2, 3]
    queue.append(4)
    queue.append(5)
    
    print(queue)
    
    #-----------------------------------
    
    # 큐 맨 앞 데이터 삭제
    queue.pop(0)     # 1 삭제
    queue.pop(0)     # 2 삭제
    
    print(queue)
    ```
    

- **단점 : 성능 문제**
    
    단, 이 방법은 성능적으로 문제가 있다. **list 자료형은 무작위 접근에 최적화된 자료 구조이기 때문이다.** `pop(0)`이나 `insert(0, x)`는 **O(n)**의 시간 복잡도를 가지고 있어 담는 데이터 개수가 많아질수록 확연히 느려지게 되는데, 이는 첫번째 데이터를 제거하면 그 뒤에 모든 데이터를 앞으로 한 칸씩 당기고 맨 앞에 데이터를 삽입하려면 모든 데이터를 한 칸씩 미는 연산이 내부적으로 포함되어 있기 때문이다.
    

<br/>

### 2. Collections 모듈의 deque 사용

- **구현**
    
    <aside>
    💡 **데크(=디큐, deque)**는 dobule-ended queue의 약자로 데이터를 양방향에서 추가하고 제거할 수 있는 자료구조다. 스택과 큐의 각 특징이 뚜렷하지만 별도로 구분해 사용하기 번거로운데, deque는 두 자료형의 특징을 모두 갖는다.
    
    Double Ended Queue를 직역하면 ‘양 끝이 앞이 되는 큐’이지만, 데크는 큐의 성질만 갖는 것이 아니라 스택과 큐의 성질을 모두 갖는다.
    
    즉, 양쪽 끝에서 삽입과 삭제가 모두 가능한 자료구조다.
    
    </aside>
    
    파이썬의 **collections** 모듈에서 **deque**를 불러와 사용한다.  파이썬의 리스트와 유사하지만 `Collections.deque`의 연산인 `appendleft()`와 `popleft()` 의 시간복잡도를 살펴보면 앞뒤에서 데이터를 처리하는 속도가 모두 **O(1)**로 매우 빠른 것을 알 수 있다. 이는 내부적으로 doubly linked list로 구현되어있기 때문이다.  따라서 앞서 리스트로 구현한 큐 구조보다 성능이 뛰어나다.
    
    ```python
    from collections import deque
    
    # deque 선언-----------------------------
    dq = deque([])
    
    # deque에 데이터 추가
    dq.append(1)
    dq.append(2)
    dq.append(3)
    dq.append(4)
    
    print(dq)
    
    # deque의 맨 앞 원소 제거------------------
    print(dq.popleft())   # 1 출력 및 제거
    print(dq.popleft())   # 2 출력 및 제거
    dq.popleft()          # 3 제거
    (dq.popleft))         # 4 제거
    
    print(dq)
    ```
    
      *  참고 : duque 객체에서 지원하는 데크 메소드는 다음과 같다.
    
    | 메소드 | 내용 | 응용 |
    | --- | --- | --- |
    | deque() | 초기화 함수 | deq = deque() : 빈 데크
    deq = deque([1,2,3]) : 원소가 있는 데크
    deq = deque(maxlen=5) : 최대 길이 명시 |
    | append(x)
    appendleft(x) | x를 덱의 오른쪽에 삽입한다.
    x를 덱의 왼쪽에 삽입한다. | deq.append(3) : 뒤에 원소 삽입deq.appendleft(3) : 맨 앞에 원소 삽입 |
    | pop()
    popleft() | 오른쪽에서부터 제거와 반환
    덱의 가장 왼쪽에 있는 원소 | deq.pop() : 오른쪽 원소 제거
    deq.popleft() : 왼쪽 원소 제거 |
    | clear() | 모든 원소를 지운다. | deq.clear() |
    | len() | 원소 수 반환 | len(deq) |
- 단점
    
    무작위 접근의 시간복잡도가 O(n)이고, 내부적으로 linked list를 사용해 N번째 데이터에 접근하려면 N번 순회가 필요하다.
    

### 3. Queue모듈의 queue 메소드 활용

- 구현
    
    파이썬의 **Queue 모듈**에서는 **큐(Queue)**, **우선순위큐(PriorityQueue)**, **스택(LifoQueue)**를 제공하고 있습니다. 특히 큐 모듈은 스레드 환경을 고려하여 작성되어 있기 때문에 여러 스레드들이 동시에 큐(Queue), 우선순위큐(PriorityQueue), 스택(LifoQueue)과 같은 객체에 접근하여 작업을 수행하여도 정상적으로 동작하는 것을 보장합니다. 여기선 기본적인 Queue를 사용하여 쉽게 Queue를 활용해 보도록 하겠습니다.
    
    ```python
    from queue import queue
    
    # Queue 선언
    que = Queue()
     
    # q에 데이터 추가
    que.put(1)
    que.put(2)
    que.put(3)
    que.put(4)
     
    # que에서 맨 앞 원소 제거
    print(que.get()) # 1
    print(que.get()) # 2
    print(que.get()) # 3
    print(que.get()) # 4
    ```
    
      *  참고 : 기타 queue 모듈에 정의된 클래스 정보는 다음과 같다.
    
    | 클래스 | 내용 |
    | --- | --- |
    | queue.Queue(maxSize) | 선입선출 => 큐 |
    | queue.LifoQueue(maxsize) | 후입선출 방식 큐 => 스택 |
    | queue.PriorityQueue(maxsize) | 데이터마다 우선순위를 주고 우선순위가 높은 순으로 데이터를 출력하는 우선순위 큐 (우선순위 숫자가 작을 수록 높은 순위) |
    | queue.SimpleQueue | 입력제한이 없는 선입선출 큐 |
    
    * 각 클래스는 다음 메소드를 동일하게 사용한다.
    
    - qsize() : 큐 객체에 등록된 아이템 갯수 반환
    - put(n) : 큐 객체에 아이템 등록
    - put_nowait : 블로킹 없이 큐 객체에 아이템 등록
    - get : 아이템 1개 반환
    - get_nowait() : 블로킹없이 큐 객체에 들어있는 아이템 반환

<br/><br/>

## 큐의 종류

---

### 선형 큐 (Linear Queue)

선형 큐(Linear Queue)는 가장 기본적인 형태의 큐(Queue)이다.  선형 큐에는 단점이 존재하는데, 삽입 및 삭제를 반복하다보면 rear가 맨 마지막 인덱스를 가리키고, 앞에는 비어있을 수도 있지만 이를 꽉 찼다고 인식한다는 점이다. 

![Untitled 1](https://user-images.githubusercontent.com/100582309/157604657-8200f56b-a5b0-48af-939c-307fa573a70b.png)
이는 실제 데이터는 삭제 때마다 한 칸 앞으로 이동시키지 않았고, 인덱스 단위로 큐의 연산을 진행했기 때문이다. 이에 생겨난 아이디어가 원형 큐이다. 

<br/>

### **원형 큐 (=환형 큐, Circular Queue)**

![Untitled 2](https://user-images.githubusercontent.com/100582309/157604598-354684fa-096b-417c-b01f-83689a4b4dd4.png)


원형 큐는 선형 큐와 마찬가지로 1차원 배열로 구현하지만, 논리적으로 배열의 처음과 끝이 연결되어 있는 것으로 생각하는 것이다.

원형 큐에서는 front와 rear 의 개념이 약간 변경된다. 먼저 초기 값은 -1이 아닌 0이다.

> front는 항상 큐의 첫 번째 요소의 하나 앞을, rear는 마지막 요소를 가리킨다.
> 
> - 처음에 front, rear는 모두 0이고 삽입 시에는 rear가 먼저 증가되고, 증가된 위치에 새로운 데이터가 삽입된다.
> - 또 삭제 시에도 front가 먼저 증가된 다음, 증가된 위치에서 데이터를 삭제한다.

원형 큐 구현

```python
class CircularQueue:

#큐 초기화
    def __init__(self, n):
        self.maxCount = n
        self.data = [None] * n
        self.count = 0
        self.front = -1
        self.rear = -1'

# 현재 큐 길이를 반환
    def size(self):
        return self.count

# 큐가 비어있는지 
    def isEmpty(self):
        return self.count == 0

# 큐가 꽉 차있는지
    def isFull(self):
        return self.count == self.maxCount

# 데이터 원소 추가
    def enqueue(self, x):
        if self.isFull():
            raise IndexError('Queue full')
        
        self.rear = (self.rear + 1) % self.maxCount
        self.data[self.rear] = x
        self.count += 1

#데이터 원소 제거
    def dequeue(self):
        if self.isEmpty():
            raise IndexError('Queue empty')

        self.front = (self.front + 1) % self.maxCount
        x = self.data[self.front]
        self.count -= 1
        return x

# 큐의 맨 앞 원소 반환
    def peek(self):
        if self.isEmpty():
            raise IndexError('Queue empty')
        return self.data[(self.front + 1) % self.maxCount]
```

```python
#연산의 정의
size() : 현재 큐에 들어 있는 데이터 원소의 수를 구함
isEmpty() : 현재 큐가 비어 있는지를 판단
isFull() : 큐에 데이터 원소가 꼭 차 있는지를 판단
qneueue(x) : 데이터 원소 x를 큐에 추가
dequeue() : 큐의 맨 앞에 저장된 데이터 원소를 제거(또한, 반환)
peek() : 큐의 맨 앞에 저장된 데이터 원소를 반환(제거하지 않음)
```

<br/>

### **우선순위 큐(Priority Queue)**

일반적인 큐(Queue)는 First in-First Out 구조이나, 우선순위 큐(Priority Queue)는 들어간 순서에 상관없이 우선순위가 높은 데이터가 먼저 나오는 것을 말한다. 우선순위 큐는 힙(Heap)이라는 자료구조를 가지고 구현할 수 있다. 

![Untitled 3](https://user-images.githubusercontent.com/100582309/157604703-cb4c729d-c215-46f0-8e0b-2a541bf08d2c.png)


<br/>

- 참고자료
    
    [https://velog.io/@keemtj/자료구조-큐Queue](https://velog.io/@keemtj/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%81%90Queue)
    
     [https://devuna.tistory.com/22](https://devuna.tistory.com/22)
    
    [https://blog.naver.com/dsz08082/222559458327](https://blog.naver.com/dsz08082/222559458327)
    
    [https://daimhada.tistory.com/107](https://daimhada.tistory.com/107)
    
    [https://airsbigdata.tistory.com/141](https://airsbigdata.tistory.com/141)