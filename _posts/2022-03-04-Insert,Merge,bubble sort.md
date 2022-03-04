---
layout: post
title:  "Insertion, Merge, Bubble Sort"
categories: Algorithm
---

# Insertion Sort, Merge Sort, Bubble Sort



## Insertion Sort

ex) 새로운 카드를 기존의 정렬된 카드 사이의 올바른 자리를 찾아 삽입한다. → 새로 삽입될 카드의 수만큼 반복하게 되면 전체 카드가 정렬됨 

= 두번째 수부터 시작해서 왼쪽의 자료들과 비교하여 삽입할 위치를 찾는 방법

= 자료 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교하여 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘 (두번째 자료부터 시작)

= 즉, 두 번째 자료는 첫 번째 자료, 세 번째 자료는 두 번째와 첫 번째 자료, 네 번째 자료는 세 번째, 두 번째, 첫 번째 자료와 비교한 후 자료가 삽입될 위치를 찾는다. 자료가 삽입될 위치를 찾았다면 그 위치에 자료를 삽입하기 위해 자료를 한 칸씩 뒤로 이동시킨다

⇒ 대부분의 레코드가 이미 정렬되어 있는 경우에 효율적!

![Untitled](/public/img/Algorithm/Insertion  5892a/Untitled.png)

![Untitled](/public/img/Algorithm/Insertion  5892a/Untitled%201.png)

INSERTION-SORT(A) : 

    for j ← 2 to length[A]

        do key ← A[j]

            Insert A[j] into the sorted sequence A[1,..,j-1].

            i ← j-1

            while i > 0  and A[i] > key

                do A[i+1] ← A[i]

                    i ← i-1

            A[i+1] ← key

- n input elements → (n-1) comparisons
    1. Best Case : 이미 모두 정렬되어있는 경우. 한번씩만 비교하면 되므로 ((n-1) comparisons) O(n)
    2. Worst Case : (n-1) + (n-2) + ... + 2 + 1 = n(n-1)/2 ⇒ O(n^2) ⇒ 평균

## Merge-Sort Algorithm

= 안정 정렬에 속하며, 분할 정복 알고리즘에 속한다. (분할정복→ 대개 순환 알고리즘을 통해 구현한다)

= 하나의 리스트를 두개의 균등한 크기로 분할하고, 각 부분 리스트를 정렬한 후 다시 합한다.추가적인 리스트가 필요함.

🔽

MERGE-SORT(A,p,r) : 

    if p < r

        then q ← [(p+r)/2]       #Divide (입력배열을 두 개의 같은 크기의 부분배열로 분할)

            MERGE-SORT(A,p,q)    #Conquer (부분배열을 정렬)

            MERGE-SORT(A,q+1,r)  #Conquer (부분배열을 정렬)

            MERGE(A,p,q,r)       #Combine (하나의 배열로 합병)

![Untitled](/public/img/Algorithm/Insertion  5892a/Untitled%202.png)

MERGE(A,p,q,r) :

    n1← q-p+1

    n2← r-q

    create arrays L[1,...,n1+1] and R[1,...,n2+1]

    for i ← 1 to n1

        do L[i] ← A[p+i-1]

    for j ← 1 to n2

        do R[j] ← A[q+j]

    L[n1+1] ← $\infin$

    R[n2+1] ← $\infin$

    i ← 1

    j ← 1

    for k ← p to r

        do if L[i] ≤ R[j]

            then A[k] ← L[i]

                i ← i+1

            else A[k] ← R[j]

                j ← j+1

![DDFAFCBA-7BA1-4EEE-8CCD-006E22EC832A.jpeg](/public/img/Algorithm/Insertion  5892a/DDFAFCBA-7BA1-4EEE-8CCD-006E22EC832A.jpeg)

- 시간복잡도

![Untitled](/public/img/Algorithm/Insertion  5892a/Untitled%203.png)

⇒ n  * logn ⇒ O(nlogn)

## Bubble Sort

= 서로 인접한 두 원소를 검사하여 정렬하는 알고리즘

= 1회전을 수행하고 나면 가장 큰 데이터가 맨 뒤에 오게된다. → 2회전에서는 마지막꺼를 제외하고 정렬, → 정렬을 1회전 수행할 때마다 정렬에서 제외되는 데이터가 하나씩 늘어난다.

![Untitled](/public/img/Algorithm/Insertion  5892a/Untitled%204.png)

BUBBLESORT(A) : 

    for i ← 1 to length[A]

        do for j ← length[A] downto i+1

            do if A[j] < A[j-1]

                then exchange A[j] <-> A[j-1]

(최솟값을 맨앞에 오게하는 코드)

- 수행횟수 : $\sum_{i=1}^{n}(i-1) = n(n-1)/2$
- 시간복잡도 : $O(n^2)$
- 구현이 매우 간단하다.

**

단순하지만 비효율적인 정렬 방법 : Insertion, Bubble Sort

복잡하지만 효율적인 정렬 방법 : Heap, Quick, Merge Sort