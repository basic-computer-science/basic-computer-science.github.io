---
layout: post
title:  "[Data Structure] Priority Queue and Heap"
date:   2022-03-11 19:00:00 +0900
categories: DataStructure
---

## **Questions**
---
- **Priority Queue의 동작 원리가 어떻게 되나요? (N사 전화면접)**
- **큐 / 우선순위 큐의 개념에 대해 설명해보세요.**

<br/>

# [자료구조] Priority Queue and Heap

# Priority Queue

> 우선순위 큐
> 

---

큐의 동작 방식은 기본적으로 FIFO(선입선출 방식)이다.

그러나 우선순위 큐는 우선순위가 가장 높은 데이터부터 먼저 삭제하는 자료구조이다. 

![priority_queue](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9a9d36cd-d6da-469c-a844-d9b4732f1a29/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220512%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220512T090552Z&X-Amz-Expires=86400&X-Amz-Signature=6a63ed6de6ba6d8235eb160bb66dc640d59f07242c38184e19e9c2d67d3d155a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject){: width="800"}

<Br>

### 특징

- 모든 항목에는 우선순위가 있다.
- 우선순위가 높은 순서대로 먼저 삭제된다.
- 우선순위가 같은 경우면 선입선출 방식으로 삭제된다.

<br>

### 기본 동작

- `enqueue()`  :  queue에 새 요소를 삽입한다.
- `dequeue()`  :  queue에서 최대 우선 순위 요소를 삭제하고 그 값을 반환합니다.
- `peek()`  :  queue에서 최대 우선순위 요소를 반환합니다.

<br>

## 구현과 시간복잡도

일반적으로 우선순위 큐를 구현하는 방식으로는 배열, 연결리스트, 힙을 이용하여 구현하는 방식이 있다. 데이터의 개수가 N개일 때, 구현 방식에 따라 시간복잡도는 다음과 같다.

| 구현 방법 | enqueue() | dequeue() |
| --- | --- | --- |
| Unsorted Array | O(1) | O(N) |
| Unsorted Linked list | O(1) | O(N) |
| Sorted Array | O(N) | O(1) |
| Sorted Linked list | O(N) | O(1) |
| Heap | O(logN) | O(logN) |

정렬된 연결리스트를 사용하는 경우를 예시로 들어 설명해보겠다. `enQueue` 과정에서 우선순위를 비교하여 적절한 위치에 삽입한다. 자연스럽게 삽입, 삭제 과정에 따라 비교연산이 많이 일어나게 된다.

이를 해결하기 위해 Python의 PriorityQueue 클래스를 사용하거나, 보통은 **Heap 자료구조**를 많이 사용한다. 

단순히 데이터를 힙에 넣었다가 모두 꺼내는 작업은 정렬과 동일하며, 시간복잡도가 O(NlogN)이기 때문에 병합 정렬 등 다른 정렬알고리즘과 동일하게 빠르다고 표현할 수 있다. 따라서 이 알고리즘을 힙 정렬 알고리즘이라고 부른다.

<br>

### 쓰임새

데이터를 자료구조에 넣었다가 우선순위가 높은 순서로 처리하고 싶을 때에 사용된다.

![우선순위 큐 활용](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/eb9a6106-503a-448a-a365-1841d42bead6/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220512%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220512T090855Z&X-Amz-Expires=86400&X-Amz-Signature=395bc2d9cb6ee19c5f7897922e15978ec114b7edb156e92aabc7999deed64217&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)


💡 **우선순위 큐와 힙이 자주 같이 다뤄지는 이유**
<br>
힙으로 우선순위 큐를 구현할 때에는 **노드에 저장된 값** = **우선순위**로 본다. 따라서, 힙은 루트 노드에 우선 순위가 높은 데이터를 위치시키는 자료구조가 된다. 
<br>
그러므로 우선순위 큐를 구현하기에 딱 맞는 자료구조이기도 하다. 힙에 저장된 노드를 뺄 때마다 우선순위가 높은 데이터가 먼저 빠져나오기 때문이다.
{: .notice}


<br><br>

# Heap

> 힙
> 

---

- 완전 이진 트리 자료구조의 일종이다.
- 힙에서는 항상 루트 노드를 제거한다.
- **Min Heap (최소 힙)** : 루트 노드가 가장 작은 값을 가진다. 따라서 값이 작은 데이터부터 우선 순위를 가지고,  우선적으로 제거된다.
    
    ![최소힙](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/34a19960-fec1-4d3e-848c-63dbaafbec73/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220512%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220512T091137Z&X-Amz-Expires=86400&X-Amz-Signature=373c490bbad4fca85099025b2b1005a177af946481a514fc51136b6dd1e126e9&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

<br>
    
- **Max Heap (최대 힙)** : 루트 노드가 가장 큰 값을 가진다. 따라서 값이 큰 데이터부터 우선 순위를 가지고, 우선적으로 제거된다.


	![~~](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/34a19960-fec1-4d3e-848c-63dbaafbec73/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220512%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220512T091137Z&X-Amz-Expires=86400&X-Amz-Signature=373c490bbad4fca85099025b2b1005a177af946481a514fc51136b6dd1e126e9&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

<br>

### 📌 Complete Binary Tree

> 완전 이진 트리
> 

---

![image](https://user-images.githubusercontent.com/100582309/168038002-669c68e3-8d3d-40bd-8b14-e9176a4869e9.png)

- 완전 이진 트리란 루트 노드부터 시작, 왼쪽 자식 노드, 오른쪽 자식 노드 순서대로 데이터가 차례로 삽입되는 트리이다.
    
    | ![Untitled 7](https://user-images.githubusercontent.com/100582309/168037977-1c6e5f03-8430-4b09-9d9c-692dbf65e684.png)|
	|:--:|
	|원소의 개수에 따라 정해진 형태가 있다 |

    
- 마지막 레벨을 제외한 나머지 노드가 꽉 차있어야 하며(마지막 레벨을 제외하고 포화 이진 트리이어야 하며), 마지막 레벨의 노드는 왼쪽 부터 차있어야 한다.

<br><br>


# Heap의 구현

---

## 삽입

- 삽입되는 새로운 노드를 우선순위가 가장 낮다는 가정을 하고 ‘맨 끝 위치’에 저장한다. 이때, 맨 끝 위치에 저장한다는 것도 완전 이진트리의 규칙 안에서 이루어져야 한다.
    
    |![Untitled 8](https://user-images.githubusercontent.com/100582309/168037960-9cc732f4-0fce-403f-87c4-789b6afca6ea.png)|
	|:--:|
	| 예제 1. 최소 힙에서 3 삽입 |
    
    
- 그리고 부모 노드와 우선순위를 비교해서 필요하면 위치를 바꾼다. 위치가 전부 적절할 때까지 반복한다.
    
    ![Untitled 9](https://user-images.githubusercontent.com/100582309/168037963-fa71d140-b257-4726-bcfd-c1824dfe5bce.png)

    
- 최대 힙은 반대로 부모와 비교해 자식이 크면 서로 자리바꿈 하면 된다.
- O(logN)의 시간복잡도로 힙 성질을 유지하도록 할 수 있다.

	![Untitled 10](https://user-images.githubusercontent.com/100582309/168037967-f69f1601-707e-4911-85cb-9dba64eade16.png)

<br>


## 구성함수 Heapify

 : heap을 구성하기 위한 함수. 상향식과 하향식이 있다.

  ex) **`Min-Heapify()`** : 최소 힙 구성 함수

(상향식) 부모노드로 거슬러 올라가며, 부모보다 자신의 값이 더 작은 경우 위치를 교체한다.

![Untitled 11](https://user-images.githubusercontent.com/100582309/168037969-8463e160-334c-4c95-9d67-481db28ba096.png)

<br>

## 삭제

**우선순위 큐**의 **`pop`** = **가장 우선순위가 높은 데이터 반환 = heap**의 **`루트 노드를 반환(삭제)`**

힙에서 가장 우선순위가 높은 데이터는 루트노드인데, 이 루트 노드를 삭제하면서 힙의 구조를 그대로 유지하는 것이 관건이다.

- 원소가 제거될 때에도 O(logN)의 시간복잡도로 힙 성질을 유지하도록 할 수 있다.

![Untitled 12](https://user-images.githubusercontent.com/100582309/168037973-e2bedccf-8e69-4c35-a266-022e6095ee9e.png)

<br><br>


## Python 구현

Python, C++, Java 등에서 표준 라이브러리를 통해 힙 자료구조를 제공하고 있다.

Python의 경우 heap 라이브러리를 import해서 사용할 수 있다.


💡 프로그래밍 언어마다 기본적으로 제공하는 heap 라이브러리가 최소 힙인지, 최대 힙인지 다를 수 있다. Python에서는 최소 힙이다.
<br>
만약 Python에서 최대 힙이 필요하다면, 데이터를 넣을 때와 뺄 때 데이터에 -를 붙여서 활용하면 최대 힙이 된다.
{: .notice}

<br>

### 우선순위 큐 라이브러리를 활용한 힙 정렬 구현 예제

```python
import sys
import heapq
input = sys.stdin.readline

def heapsort(iterable):
	h = []
	result = []                    # 모든 원소를 차례대로 힙에 삽입
	for value in iterable:
		heapq.heappush(h, value)     # 힙에 삽입된 모든 원소를 차례대로 꺼내어 담기
	for i in range(len(h)):
		result.append(heapq.heappop(h))
	return result

n = int(input())
arr = []

for i in range(n):
	arr.append(int(input()))

res = heapsort(arr)

for i in range(n):              # 결과적으로 힙 정렬한 결과 출력해보면 오름차순 정렬된 결과가 출력됨
	print(res[i])
```

<br><br>


### 참고자료

- [https://yoongrammer.tistory.com/81](https://yoongrammer.tistory.com/81)
- 동빈나 Youtube : [https://www.youtube.com/watch?v=AjFlp951nz0](https://www.youtube.com/watch?v=AjFlp951nz0)
- [https://chanhuiseok.github.io/posts/ds-4/](https://chanhuiseok.github.io/posts/ds-4/)