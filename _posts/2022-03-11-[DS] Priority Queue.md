---
layout: post
title:  "[Data Structure] Priority Queue"
date:   2022-03-07 19:00:00 +0900
categories: DataStructure
---

## **Questions**
---
- **Priority Queue의 동작 원리가 어떻게 되나요? (N사 전화면접)**
- **큐 / 우선순위 큐의 개념에 대해 설명해보세요.**

<br/>

# Priority Queue (우선순위 큐)

> 우선순위를 가진 항목들을 저장하는 Queue
> 
> 
> 선입선출방식(FIFO)이 아니라 **우선순위가 높은 순서대로** 먼저 나감 (But, **우선순위가 같은 경우면 선입선출**)
> 

---

![Untitled 12](https://user-images.githubusercontent.com/100582309/160956005-f913402e-72f2-416e-9519-36c2a90ab3f2.png)


<br/>

## 구현 방식

- 리스트를 이용
- 우선순위 Queue 라이브러리 사용

<br/>

## 기본 연산

삽입 : `enQueue` - 삽입된 원소를 우선순위에 맞는 위치에 삽입

삭제 : `deQueue` - 우선순위가 가장 높은, 가장 앞의 원소를 삭제

![Untitled 13](https://user-images.githubusercontent.com/100582309/160956006-3948bd62-b058-4d57-a45a-8c28419bad2c.png)




# 우선순위 큐 : 리스트를 이용한 구현

---

- `enQueue` (삽입) 과정에서 우선순위를 비교하여 적절한 위치에 삽입
- 최고 우선순위의 원소는 리스트의 가장 앞에 위치하게 됨

- 문제점 : 리스트를 사용하므로 삽입/삭제 연산 시 원소의 재배치 발생
    
                       → 소요 시간/메모리 큼
    
- 해결 : 연결리스트로 구현 가능 (But, 비교 연산 많이 발생)
    
                        → **PriorityQueue(maxsize) 클래스** 사용   or      **heap 자료구조** 사용
    

# BFS(너비 우선 탐색)

---

## 그래프 탐색 방법

> **DFS** (Depth First Search, 깊이 우선 탐색) : **Stack** 활용
> 
> 
> **BFS** (Breadth First Search, 너비 우선 탐색) : **Queue** 활용
> 

## BFS (너비 우선 탐색)

- 탐색 시작점에 인접한 정점들을 먼저 모두 차례로 방문 후 방문했던 정점을 시작점으로 하여 다시 인접한 정점들을 모두 차례로 방문하는 방식
- 인접한 정점들에 대해 탐색을 한 후 차례로 다시 BFS를 진행해야 하므로 **FIFO(선입선출) 형태의 자료 구조인 queue를 활용**합니다.


## BFS 탐색 과정

: 맨 위의 노드로부터 가까운 노드 순으로 검색

### 입력 파라미터

→ 이를 계속해서 반복하면 **BFS가 동작**함

```python
def BFS(G, v) :           # 그래프 G, 탐색 시작점 v
	visited = [0] * n       # n : 정점의 개수
	queue = []             # 큐 생성
	queue.append(v)        # 시작점 v를 큐에 삽입

	while queue :              # 큐가 비어있지 않은 경우
		t = queue.pop(0)         # 큐의 첫 번째 원소 반환
		if not visited[t] :      # 방문하지 않은 곳이라면
			visited[t] = True      # 방문한 것으로 표시
			visit(t)
		for i in G[t] :          # t와 연결된 모든 선에 대해
			if not visited[i] :    # 방문되지 않은 곳이라면
				queue.append(i)      # 큐에 넣기
```


## 알고리즘 추적

자료구조 : Visited 리스트 (방문 여부 정보 저장), Q 리스트(BFS의 진행상황 저장)

### 1. **시작점 enQueue**

![Untitled 14](https://user-images.githubusercontent.com/100582309/160956008-4ab6da60-c982-4bb5-84a8-f53ad9bf6188.png)

- Q에 `enQueue(A)`

### 2. **A점부터 시작**

![Untitled 15](https://user-images.githubusercontent.com/100582309/160956010-6ef14a0b-4815-47c8-8086-c8466d818007.png)

- Q에서 `deQueue()` : A 삭제
- `Q[0] = T`   : A 방문한 것으로 표시
- `enQueue(B, C, D)` : A의 인접점을 큐에 삽입


### 3. B점부터 탐색 진행

![Untitled 16](https://user-images.githubusercontent.com/100582309/160956013-88212944-3243-4d05-9685-273be88f9fe7.png)

- Q에서 `deQueue()` : B 삭제
- `Q[1] = T`   : B 방문한 것으로 표시
- `enQueue(E,F)` : B의 인접점을 큐에 삽입

### 4. C점부터 탐색 진행


![Untitled 17](https://user-images.githubusercontent.com/100582309/160956015-a85b8cdd-dac6-4c64-9ec8-42c83f9ec867.png)

- Q에서 `deQueue()` : C 삭제
- `Q[2] = T`   : C 방문한 것으로 표시
- `enQueue()` : C의 인접점을 큐에 삽입해야 하지만 없으므로 원소 추가되지 않음

### 5. D점부터 탐색 진행

![Untitled 18](https://user-images.githubusercontent.com/100582309/160956023-7708bfb1-cc5c-4f8a-8d86-51925fda3ec0.png)

- Q에서 `deQueue()` : D 삭제
- `Q[3] = T`   : D 방문한 것으로 표시
- `enQueue(G, H, I)` : D의 인접점을 큐에 삽입


### 6. E점부터 탐색 진행

![Untitled 19](https://user-images.githubusercontent.com/100582309/160956032-80e1c6ef-329c-4092-863f-6d931e1aa482.png)

- Q에서 `deQueue()` : E 삭제
- `Q[4] = T`   : E 방문한 것으로 표시
- `enQueue()` : E의 인접점을 큐에 삽입해야 하지만 없으므로 원소 추가되지 않음

### 7~. 같은 방법으로 F, G, H, I 방문

![Untitled 20](https://user-images.githubusercontent.com/100582309/160956041-9bf75dd6-a262-4338-a6cd-459618be64dd.png)


![Untitled 21](https://user-images.githubusercontent.com/100582309/160956046-43f5977f-40ef-4bae-bc56-8ae536815699.png)

![Untitled 22](https://user-images.githubusercontent.com/100582309/160956053-e314c0e7-14bc-4b18-9736-e1b29887dc79.png)

![Untitled 23](https://user-images.githubusercontent.com/100582309/160956060-bf2432c8-d916-43cb-a9a9-88fa4ce1ebb0.png)


- **I까지 방문하고 나면 탐색을 종료함**