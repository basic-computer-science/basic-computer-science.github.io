---
layout: post
title:  "[Algorithm] Elementary Graph Algorithms -BFS, DFS, DAG, Strongly Connected Components"
categories: Algorithm
---

# Elementary Graph Algorithms -BFS, DFS, DAG, Strongly Connected Components

## 그래프 탐색

- 하나의 정점으로부터 시작하여 차례대로 모든 정점들을 하나씩 방문하는 것

![Untitled](/public/img/Algorithm/Elementary/Untitled.png)

→ BFS : A - B - C - H - D - I - J - M - E - G - K - F - L

→ DFS : A - B- C - D - E - F - G - H - I - J - K - L - M

```python
graph = {
    'A': ['B'],
    'B': ['A', 'C', 'H'],
    'C': ['B', 'D'],
    'D': ['C', 'E', 'G'],
    'E': ['D', 'F'],
    'F': ['E'],
    'G': ['D'],
    'H': ['B', 'I', 'J', 'M'],
    'I': ['H'],
    'J': ['H', 'K'],
    'K': ['J', 'L'],
    'L': ['K'],
    'M': ['H']
}
```

## Breadth-First Search (너비 우선 탐색)

- 루트노드에서 시작해서 인접한 노드를 먼저 탐색하는 방법
- 시작 정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어져 있는 정점을 나중에 방문하는 순회 방법
- 깊게 탐색하기 전에 넓게 탐색하는 방법
- 두 노드 사이의 최단 경로 혹은 임의의 경로를 찾고 싶을 때 이 방법을 선택


💡 <노드 방문 순서>

L0 = {S}   #S는 출발노드

→ L1 = L0의 모든 이웃노드들

→ L2 = L1의 이웃들 중 L0에 속하지 않는 노드들

... → Li = L(i-1)의 이웃들 중 L(i-2)에 속하지 않는 노드들

⇒ 깊이가 1인 모든 노드를 방문하고 나서 그 다음에는 깊이가 2인 모든 노드를, 그 다음에는 깊이가 3인 모든 노드를 방문하는 식으로 계속 방문하다가 더 이상 방문할 곳이 없으면 탐색을 마친다


- 특징
    - 재귀적으로 동작하지 않는다.
    - 어떤 노드를 방문했었는지 여부를 반드시 검사 해야 한다
    - BFS는 방문한 노드들을 차례로 저장한 후 꺼낼 수 있는 자료 구조인 큐(Queue)를 사용한다
        - 선입선출(FIFO) 원칙으로 탐색
        - 큐를 이용해서 반복적 형태로 구현하는 것이 가장 잘 동작한다.

🔻Python 구현

```python
def bfs(graph, start_node):
	visit = list() #방문했던 노드들을 차례대로 저장할 리스트
	queue = list() #다음으로 방문할 노드의 목록
	
	queue.append(start_node) #시작노드를 큐에 넣어준다.

	while queue: #큐의 목록이 바닥날 때까지(더이상 방문할 노드가 없을 때까지) loop를 돌려준다.
		node = queue.pop(0) #큐의 맨앞에있는 노드를 꺼내온다.
		if node not in visit: #해당노드가 아직 방문리스트에 없다면
			visit.append(node) #방문리스트에 추가해주고
			queue.extend(graph[node]) #해당 노드의 자식 노드들을 큐에 추가한다.

	return visit
```

## Depth-First Search (깊이 우선 탐색)

- 루트 노드(혹은 다른 임의의 노드)에서 시작해서 다음 분기(branch)로 넘어가기 전에 해당 분기를 완벽하게 탐색하는 방법
- 미로를 탐색할 때 한 방향으로 갈 수 있을 때까지 계속 가다가 더 이상 갈 수 없게 되면 다시 가장 가까운 갈림길로 돌아와서 이곳으로부터 다른 방향으로 다시 탐색을 진행하는 방법과 유사하다
- 넓게 탐색하기 전에 깊게 탐색하는 방법
- 모든 노드를 방문하고자 하는 경우에 이 방법을 선택한다.
- DFS가 BFS보다 좀 더 간단하다.
- 단순 검색 속도는 BFS에 비해서 느리다.


💡 <노드 방문 순서>
출발점 S에서 시작
→ 현재 노드를 visited로 mark 하고, 인접 노드들 중 unvisited 노드가 있으면 그 노드로 간다.
→ 반복
→ 인접한 노드들 중 unvisited 노드가 존재하지 않는 경우 unvisited인 이웃노드가 존재하지 않는 동안 계속해서 직전노드로 되돌아간다.
→ 다시 반복
→ 시작노드 S로 돌아오고 더 이상 갈 곳이 없으면 종료


- 특징
    - 자기 자신을 호출하는 순환 알고리즘의 형태를 가지고 있다.
    - 전위 순회를 포함한 다른 형태의 트리 순회는 모두 DFS의 한 종류이다.
    - 이 알고리즘을 구현할 때 가장 큰 차이점은, 그래프 탐색의 경우 어떤 노드를 방문했었는지 여부를 반드시 검사 해야 한다는 것이다
        - 이를 검사하지 않으면 무한 루프에 빠질 위험이 있다.

🔻Python 구현

→ queue대신 stack을 사용한다.

```python
def dfs(graph, start_node):
	visit=list()
	stack=list()
	
	stack.append(start_node)
	
	while stack:
		node=stack.pop()
		if node not in visit:
			visit.append(node)
			stack.extend(graph[node])

	return visit
```

- 시간 복잡도
    - N: 정점의 수, E: 간선의 수)
        - 인접 리스트로 표현된 그래프 : O(N+E)
        - 인접 행렬로 표현된 그래프 : O(N^2)
    - 그래프 내에 적은 숫자의 간선만을 가지는 희소그래프의 경우 인접 행렬보다 인접 리스트를 사용하는 것이 유리하다.

*참고: [https://suyeon96.tistory.com/32](https://suyeon96.tistory.com/32)

![Untitled](/public/img/Algorithm/Elementary/Untitled1.png)

## Topological Sorting (위상 정렬)

- 어떤 일을 하는 순서를 찾는 알고리즘
    - 방향 그래프에 존재하는 각 정점들의 선행 순서를 위배하지 않으면서 모든 정점을 나열하는 것
- 입력: Edge에 방향이 있으면서 순환되지 않는 그래프
    - = Directed Acyclic Geaph (DAG)


💡 1. 진입 차수가 0인 정점(즉, 들어오는 간선의 수가 0)을 선택
- 진입 차수가 0인 정점이 여러 개 존재할 경우 어느 정점을 선택해도 무방하다.
- 초기에 간선의 수가 0인 모든 정점을 큐에 삽입
2. 선택된 정점과 여기에 부속된 모든 간선을 삭제
- 선택된 정점을 큐에서 삭제
- 선택된 정점에 부속된 모든 간선에 대해 간선의 수를 감소
3. 위의 과정을 반복해서 모든 정점이 선택, 삭제되면 알고리즘 종료


![Untitled](/public/img/Algorithm/Elementary/Untitled2.png)

## Strongly Connected Components (강한 결합 요소)

- 그래프 안에서 강하게 결합된 정점 집합
- 같은 SCC에 속하는 두 정점은 서로 도달이 가능하다.
- 사이클이 발생하는 경우 무조건 SCC에 해당한다.

![Untitled](/public/img/Algorithm/Elementary/Untitled3.png)

[출처]

[https://gmlwjd9405.github.io/2018/08/15/algorithm-bfs.html](https://gmlwjd9405.github.io/2018/08/15/algorithm-bfs.html) [https://gmlwjd9405.github.io/2018/08/14/algorithm-dfs.html](https://gmlwjd9405.github.io/2018/08/14/algorithm-dfs.html)

[https://itholic.github.io/python-bfs-dfs/](https://itholic.github.io/python-bfs-dfs/)

[https://gmlwjd9405.github.io/2018/08/27/algorithm-topological-sort.html](https://gmlwjd9405.github.io/2018/08/27/algorithm-topological-sort.html)

[https://blog.naver.com/ndb796/221236952158](https://blog.naver.com/ndb796/221236952158)