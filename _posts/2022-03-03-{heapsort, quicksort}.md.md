---
layout: post
title:  "Heap sort, Quick sort"
date:   2022-02-28 19:31:29 +0900
categories: Algorithm
---

# Heap Sort, Quick Sort

## 💡Heapsort

: 주어진 데이터를 힙 자료구조로 만들어서 최소값 또는 최대값부터 하나씩 꺼내 정렬하는 알고리즘

- 완전 이진트리
- 최댓값, 최솟값을 쉽게 추출할수있다
- 부모노드가 자식노드보다 크거나 같음 (Max Heap : 부모≥자식, Min Heap : 부모≤자식)
    
    ![Untitled](Heap%20Sort,%2081f99/Untitled.png)
    

- Arrays
    
    ⇒ PARENT(i)
    	return [i/2]
        LEFT(i)
    	return 2i
        RIGHT(i)
    	return 2i+1	
    

✅Heapify Algorithm : 힙생성 알고리즘 (하나의 노드를 제외하고는 heap property를 만족한다는 가정하에) 

⇒ MAX-HEAPIFY

![6A70F12B-E1D3-4B3A-BFAB-6FB83DB94203.jpeg](Heap%20Sort,%2081f99/6A70F12B-E1D3-4B3A-BFAB-6FB83DB94203.jpeg)

<Pseudocode>

*MAX-HEAPIFY(A,i)
1	l ← LEFT(i)
2	r ← RIGHT(i)
3	if l ≤ heap-size[A] and A[l] > A[i]
4		then largest←l
5		else largest←i
6	if r ≤ heap-size[A] and A[r] > A[largest]
7		then largest ← r
8	if largest ≠ i
9		then exchangeA[i] <-> A[largest]
10			MAX-HEAPIFY(A, largest)*

Time/Comparisons : $O(logn)$

⇒ 호출횟수가 n번이므로 전체 수행시간(힙정렬) : $O(nlogn)$ 

**length[A] : 배열의 원소 갯수,

  heap-size[A] : 힙정렬이 되어있는 A배열의 원소의 개수

**heapq 모듈 

→ heapq.heapify(x) ⇒ 리스트x를 최소 힙으로 바꿔줌

→ heapq.heappop(heap) ⇒ 가장작은원소를 pop

→ heapq.heappush(item, heap) ⇒ item을heap에추가

✅Build MAX-HEAP : 입력 자료들 (배열)을 최대 힙으로 구성. leaf node보다 한단계 위의 노드부터 위로올라감.

![C0143A3E-48F5-439B-93E1-73BAAF68FDD9.jpeg](Heap%20Sort,%2081f99/C0143A3E-48F5-439B-93E1-73BAAF68FDD9.jpeg)

*BUILD-MAX-HEAP(A) :*

*heap-size[A] ← length[A]*

*for i ← [length[A]/2] downto 1*

*do MAX-HEAPIFY(A, i)*

✅Heapsort Algorithm : 최대 힙의 루트노드부터 차례대로 꺼내어 저장. 

*HEAPSORT(A):*

*BUILD-MAX-HEAP(A)*

*for i ← length[A] downto 2*

*do exchange A[1] <->A[i]*

*heap-size[A] ← heap-size[A] -1*

*MAX-HEAPIFY(A,1)*

## 💡Quick Sort

: 매우 빠른 정렬속도를 자랑하는 분할 정복 알고리즘. 리스트를 비균등하게 분할.

- 입력 자료들 중 하나의 요소(=pivot)를 선택한 후, 피벗을 기준으로 피벗보다 작은요소/큰요소를 나눔 → 분할되지 않을 때 까지 반복
- Divide 분할: A[p,...,r]을 two subarrays A[p,...,q-1], A[q+1,...,r] 로 분할
- Conquer 정복 : 퀵 정렬을 재귀호출해서 A[p, ..., q-1]과 A[q+1, ..., r]두 부분배열로 정렬
- Combine 결합 : 부분 배열이 이미 정렬되어있으므로 합치는 작업 필요하지 않음

<Pseudocode>

*1) Partition(A,p,r)*

*x ← A[r]*

*i ← p-1*

*for j ← p to r-1*

*do if A[j] ≤ x*

*then i ← i+1*

*exchange A[i] <-> A[j]*

*exchange A[j+1] <-> A[r]*

*return i+1*

![Untitled](Heap%20Sort,%2081f99/Untitled%201.png)

*2) QUICKSORT(A,p,r)*

*if p<r*

*then q ← Partition(A,p,r)*

*QUICKSORT(A,p,q-1)*

*QUICKSORT(A,q+1,r)*

- Best case (n/2 , p , n/2 로 나뉜경우)
    - T(n) = 2T(n/2) + $\theta(n)$ = $\theta(nlogn)$
- Worst case (n-1, p, 0 으로 나뉜경우)
    - T(n) = T(n-1) + T(0) + $\theta(n)$ = T(n-1) + $\theta$(n) = $\theta(n^2)$
- Average Case : $\theta(nlogn)$