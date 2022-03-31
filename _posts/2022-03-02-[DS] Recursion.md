---
layout: post
title:  "Recursion"
date:   2022-03-02 19:00:00 +0900
categories: DataStructure
---

<br/>

# 재귀함수란?

---

재귀함수란 어떤 함수 func(n)이 있다고 할 때, 이 함수 내부에서 자기 자신을 다시 호출하는 함수를 의미한다. 

<br/>

## 1) 재귀함수의 특징

---

재귀 함수는

1. 자기 자신을 호출하는 부분
2. 일정 조건을 만족하면 호출을 정지하고 어떤 값을 return하는 부분

이 두 분으로 나뉘어진다.

그리고 함수 호출은 메모리의 스택 영역에 쌓이게 되는데, 스택은 후입선출(LIFO)방식으로 데이터를 저장하는 자료구조임을 우리는 알고 있다. 따라서, 아래 함수를 실행했을 때 스택 영역에 쌓이는 함수는 다음과 같다.

```python
def sol(n):

	if n == 5:
		return

	else:
		return sol(n+1)

sol(0)
```

![Untitled](https://user-images.githubusercontent.com/100582309/157596762-a3037529-b459-449c-8334-0d2fc2686f18.png)


<aside>
👉 재귀함수 호출마다 새로운 스택이 생긴다.

</aside>

<aside>
👉 재귀 함수의 단점은 n이 증가하면 시간 복잡도 O(2n)가 가파르게 증가한다는 점입니다.

(→ 이러한 문제를 해결하기 위해 메모이제이션(Memoization)을 적용할 수 있습니다.)

</aside>

<aside>
‼️ 재귀함수 호출 시 입력(인자)값이 특정 패턴을 반복하거나, 변화가 없으면 무한히 반복하게 된다.

▶재귀함수 설계 시 입력 값이 종료 조건으로 수렴하는지 꼭 확인해야 한다.

</aside>

<br/>

## 2) 재귀함수 짜는 법

---

1.  자신을 호출하는 것을 정지시키는 조건을 만들어야 한다.

1. 한 동작마다 해야하는 것들을 정의해서, 함수 내부에 써넣는다.
2. 다음 함수를 호출할 때, 파라미터로 어떤 것들을 넘겨줘야 할 지 정해야 한다.

<br/>

# 재귀함수의 예시

---

## 1. 팩토리얼

```python
def factorial(n):
    if n == 1:      # n이 1일 때
        return 1    # 1을 반환하고 재귀호출을 끝냄
    return n * factorial(n - 1)    # n과 (factorial 함수에 n-1을 넣어서 반환된 값)을 곱함
```

<br/>

## 2. 거듭제곱

```python
def solution2(x,n):
	if n == 0:
		return x
	else:
		return x * solution2(x,n-1)
```

<br/>

## 3. 피보나치수열

- 피보나치 수 : 첫번째와 두번째 값이 1이고 다음부터는 그 전의 수와 그 전전의 수를 더하는 방식

첫 번째 값이 0으로 시작하는 경우도 있으며 다음과 같은 형태의 수열이다.

                                                           (0), 1, 1, 2, 3, 5, 8, 13, ...

```python
def fib(n):

	if n == 0:
		return 0

	elif n == 1 or n == 2:
		return 1

	else:
		return fib(n - 1) + fib(n - 2)
```

<br/>

## 4. 하노이의 탑

하노이의 탑 문제는 재귀 호출을 이용하여 풀 수 있는 가장 유명한 예제 중의 하나이다. 그렇기 때문에 프로그래밍 수업에서 재귀 알고리즘 예제로 많이 사용한다.

```python
# 입력 : 옮기려는 원반의 개수 n
# 옮길 원반이 현재 있는 출발점 기둥 from_pos
# 원반을 옮길 도착점 기둥 to_pos
# 옮기는 과정에서 사용할 보조 기둥 aux_pos
# 출력 : 원반을 옮기는 순서

def hanoi(n, from_pos, to_pos, aux_pos):

	if n == 1 :         # 원반 한 개를 옮기는 문제면 그냥 옮기면 됨
		print(from_pos, '->', to_pos)
		return

	hanoi(n - 1, from_pos, aux_pos, to_pos)   	# 원반 n-1개를 aux_pos로 이동(to_pos를 보조 기둥으로)

	print(from_pos, '->', to_pos)     # 가장 큰 원반을 목적지로 이동
	hanoi(n - 1, aux_pos, to_pos, from_pos)    	# aux_pos에 있는 원반 n-1개를 목적지로 이동 (from_pos를 보조 기둥으로)
```

```python
print('n = 1')
hanoi(1, 1, 3, 2) # 원반 한 개를 1번 기둥에서 3번 기둥으로 이동
```

n = 1

1 -> 3

```python
print('n = 2')
hanoi(2, 1, 3, 2) # 원반 한 개를 1번 기둥에서 3번 기둥으로 이동
```

n = 2

1 -> 2

1 -> 3

1 -> 3

<br/>

이 문제의 핵심은 3가지 원기둥을 1,2,3 이라고 정하는데 이 뜻은 시작,보조,목표 기둥 3가지이다.

이 기둥 번호들을 통해서 **원반이 어떻게 이동되는지를 기둥 번호로 나타내는 것!**

<br/>

<aside>
💡 하노이의 탑 알고리즘 핵심

1. 제일 큰 원반을 제외한 나머지 원반들을 보조 기둥으로 보낸다는 것 (재귀로 구현)
2. 그 다음 시작 기둥에 딸랑 혼자 있는 제일 큰 원반이 처음의 목표 기둥으로 간다는 것 (이는 print로 구현)
3. 보조 기둥에 있는 원반들(n-1개의 원반들)을 그대로 목표기둥으로 보내는 것 (다시 재귀로 구현)
</aside>

<br/>

- 참고자료
    
    [https://thesauro.tistory.com/entry/자료구조-공부5-순환-반복2](https://thesauro.tistory.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B3%B5%EB%B6%805-%EC%88%9C%ED%99%98-%EB%B0%98%EB%B3%B52)
    
    [https://sikaleo.tistory.com/29](https://sikaleo.tistory.com/29)
    
    [https://sexy-developer.tistory.com/41](https://sexy-developer.tistory.com/41)
    
    [https://psychoria.tistory.com/770](https://psychoria.tistory.com/770)