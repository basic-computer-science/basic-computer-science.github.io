---
layout: post
title:  "Counting Sort, Radix Sort"
categories: Algorithm
---

# Counting Sort, Radix Sort

# <1> Counting Sort

- O(n)의 시간복잡도를 가진 아주 빠른 정렬 알고리즘
- 안정성(stable)을 가진다.
- 정렬할 배열의 원소와 숫자를 counting하여 누적시킨 후 그 값들을 정렬된 배열의 인덱스로 사용

COUNTING-SORT(A,B,n,k) : 

for i ← 0 to k

do C[i]←0

for j ← 1 to n

do C[A[j]] ← C[A[j]]+1

for i ← 1 to k

do C[i] ← C[i] + C[i-1]

for j ← n downto 1

do B[C[A[j]]] ← A[j]

C[A[j]] ← C[A[j]]-1

![2C0F1E6C-4DAC-4867-BD97-0D1E5A695E32.jpeg](\public\img\Algorithm\Counting S 82671\2C0F1E6C-4DAC-4867-BD97-0D1E5A695E32.jpeg)

C: 버켓, k: 최댓값, n: 원소갯수

- stable이란 정렬되기 전 동일한 값을 가지는 원소들의 순서가 정렬된 후에도 유지되도록 하는 것을 의미(heap, quick은 unstable)
- 최댓값에 의해 메모리가 낭비될수도있음

# <2>Radix Sort

- 자리 수를 비교해서 정렬하는 방식
- 가장 낮은 자리 수부터 비교하여 정렬 수행 (비교 연산을 하지 않음)
- 기수정렬이 올바르게 동작되기 위해 각 자리에 사용될 정렬 알고리즘이 안정적이어야 함

RADIX-SORT(A,d) : 

for i ← 1 to d

do use a sable sort to sort array A on digit i

![A564F70A-1075-4AD2-A2BB-A689052A3CAC.jpeg](\public\img\Algorithm\Counting S 82671\A564F70A-1075-4AD2-A2BB-A689052A3CAC.jpeg)

- Backend는 Counting Sort
    - 0~9 까지의 Bucket을 준비
- 시간 복잡도가 O(dn)인 매우 빠른 정렬
- 제자리 정렬이 아니므로 추가적인 메모리 공간이 필요
- A: 정렬할 배열, d: 자릿수의 갯수