---
layout: post
title:  "[Algorithm] Minimum Spanning Trees - Kruskal’s Alg, Prim’s Alg"
categories: Algorithm
---
# Minimum Spanning Trees - Kruskal’s Alg, Prim’s Alg

- Spanning Tree : 그래프 내의 모든 정점을 포함하는 트리
    - n개의 정점 - (n-1)개의 간선
- Minimum Spanning Tree (최소 신장 트리): Spanning Tree 중에서 사용된 간선들의 가중치 합이 최소인 트리

## 🔻Spanning Tree

![Untitled](/public/img/Algorithm/Minimum/Untitled.png)

- DFS, BFS을 이용하여 그래프에서 신장 트리를 찾을 수 있다.
    - 탐색 도중에 사용된 간선만 모으면 만들 수 있다.
- 하나의 그래프에는 많은 신장 트리가 존재할 수 있다.
- Spanning Tree는 트리의 특수한 형태이므로 모든 정점들이 연결 되어 있어야 하고 사이클을 포함해서는 안된다.
- 따라서 Spanning Tree는 그래프에 있는 n개의 정점을 정확히 (n-1)개의 간선으로 연결 한다.
- 사용 사례 > 통신네트워크 구축(회사 내의 모든 전화기를 가장 적은 수의 케이블을 사용하여 연결하고자 하는 경우)

## 🔻MST

![Untitled](/public/img/Algorithm/Minimum/Untitled1.png)

- 각 간선의 가중치가 동일하지 않을 때 단순히 가장 적은 간선을 사용한다고 해서 최소 비용이 얻어지는 것은 아니다.
- MST는 간선에 가중치를 고려하여 최소 비용의 Spanning Tree를 선택하는 것을 말한다.
- 즉, 네트워크(가중치를 간선에 할당한 그래프)에 있는 모든 정점들을 가장 적은 수의 간선과 비용으로 연결하는 것이다.
- 특징 3가지
    - 간선의 가중치의 합이 최소여야 한다.
    - n개의 정점을 가지는 그래프에 대해 반드시 (n-1)개의 간선만을 사용해야 한다.
    - 사이클이 포함되어서는 안된다.
- 사용 사례 > 통신망, 도로망, 유통망에서 길이, 구축 비용, 전송 시간 등을 최소로 구축하려는 경우

## 🔻MST 구현 알고리즘 (Kruskal’s Alg, Prim’s Alg)

### **Generic MST Algorithm (두 알고리즘이 가지고 있는 공통적인 특징)**

![Untitled](/public/img/Algorithm/Minimum/Untitled2.png)

1) 처음에 A = 공집합 으로 둔다.

2) 집합 A에 대해서 안전한 에지를 하나 찾은 후 이것을 A에 더한다. (MST의 규칙을 어기지 않는 에지를 찾은 후)

3) 에지의 수 (n-1)이 될 때 까지 2번을 반복한다.

▶️안전한 엣지 찾기?

1. 그래프의 정점들을 두 개의 집합 S와 V-S로 분할한 것을 cut(S, V-S)라고 부른다.
2. 에지 (u, v)에 대해서 u가 s에 속하고 v가 v-s에 속할 때 에지 (u, v)는 컷 (S, V-S)를 크로스한다고 말한다.
3. 에지들의 부분집합 A에 속한 어떤 에지도 컷(S, V-S)를 하지 cross하지 않을 때 컷 (S, V-S)는 A를 존중한다고 한다.

![Untitled](/public/img/Algorithm/Minimum/Untitled3.png)

⇒ A가 MST의 부분집합이고 (S, V-S)는 A를 존중하는 컷이라고 한다. 이 컷을 cross하는 에지들 중 가중치가 작은 에지 (u, v)는 A에 대해서 안전하다.

![Untitled](/public/img/Algorithm/Minimum/Untitled4.png)

### Kruskal’s Algorithm

- 네트워크(가중치를간선에할당한그래프)의 모든 정점을 최소비용으로 연결하는 최적해답을 구하는 것. safe 하고 light한 Edge를 반복적으로 찾아가는 기법
- 간선선택을 기반으로 한  알고리즘. 무조건 최소 간선만을 선택
- MST(최소 비용 신장 트리) 가 1) 최소 비용의 간선으로 구성됨 2) 사이클을 포함하지 않음 의 조건에 근거하여 각 단계에서 사이클을 이루지 않는 최소 비용 간선을 선택 한다.
- 과정
    1. 그래프의 간선들을 가중치의 오름차순으로 정렬한다.
    2. 정렬된 간선 리스트에서 순서대로 사이클을 형성하지 않는 간선을 선택한다.
        
        → 즉, 가장 낮은 가중치를 먼저 선택한다.
        
        → 사이클을 형성하는 간선을 제외한다.
        
    3. 해당 간선을 현재의 MST(최소 비용 신장 트리)의 집합에 추가한다.
    4. (n-1)개의 간선들이 선택되면 종료

![Untitled](/public/img/Algorithm/Minimum/Untitled5.png)

→ 다음 간선을 이미 선택된 간선들의 집합에 추가할 때 사이클을 생성하는지를 체크

→ 새로운 간선이 이미 다른 경로에 의해 연결되어 있는 정점들을 연결할 때 사이클이 형성된다.

→ 즉, 추가할 새로운 간선의 양끝 정점이 같은 집합에 속해 있으면 사이클이 형성된다 

⇒ Union Find Algorithm 사용

- 시간복잡도
    - union-find 알고리즘을 이용하면 Kruskal 알고리즘의 시간 복잡도는 간선들을 정렬하는 시간에 좌우된다.
    - 즉, 간선 e개를 퀵 정렬과 같은 효율적인 알고리즘으로 정렬한다면 Kruskal 알고리즘의 시간 복잡도는 O(elog₂e) 이 된다.

### Prim’s Algorithm

- 시작 정점에서부터 출발하여 집합을 단계적으로 확장해나가는 방법
- 정점 선택을 기반으로 한다. 이전 단계에서 만들어진 신장트리를 확장하는 방법
- 과정
    1. 시작 단계에서는 시작 정점만이 MST(최소 비용 신장 트리) 집합에 포함된다.
    2. 앞 단계에서 만들어진 MST 집합에 인접한 정점들 중에서 최소 간선으로 연결된 정점을 선택하여 트리를 확장한다.
        
        → 즉, 가장 낮은 가중치를 먼저 선택한다.
        
    3. 위의 과정을 트리가 (N-1)개의 간선을 가질 때까지 반복한다.

![Untitled](/public/img/Algorithm/Minimum/Untitled6.png)

- 시간복잡도
    - 주 반복문이 정점의 수 n만큼 반복하고, 내부 반복문이 n번 반복
    Prim의 알고리즘의 시간 복잡도는 O(n^2) 이 된다.

- Kruskal 알고리즘의 시간 복잡도는 O(elog₂e) 이므로
    - 그래프 내에 적은 숫자의 간선만을 가지는 ‘희소 그래프(Sparse Graph)’의 경우 Kruskal 알고리즘이 적합하고
    - 그래프에 간선이 많이 존재하는 ‘밀집 그래프(Dense Graph)’ 의 경우는 Prim 알고리즘이 적합하다.

출처[https://gmlwjd9405.github.io/2018/08/28/algorithm-mst.html](https://gmlwjd9405.github.io/2018/08/28/algorithm-mst.html)