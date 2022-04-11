---
layout: post
title:  "[Algorithm] Asymptotic notation"
categories: Algorithm
---

# Asymptotic notation

# Asymptotic notation and complexity

- Asymptotic Notation : 점근 표기법. 내가 만든 알고리즘이 얼마나 효율적인지를 나타내는 도구
    - 알고리즘의 계산 복잡성은 컴퓨터의 성능에 관계없이 machine independent한 방식으로 따진다.
    - 알고리즘 자체의 효율성은 데이터 개수(n)가 주어졌을 때 덧셈, 뺄셈, 곱셈 등 해당 알고리즘 수행시 필요한 ‘기본 연산(basic operation)의 횟수’로 표현하는 것이 일반적
    - 점근 표기법에서는 원 함수를 단순화하여 최고차항의 차수만을 고려. 최고차항을 제외한 모든 항과 최고차항의 계수는 무시
    - ex> 알고리즘1의 계산복잡성: n^2+n+100, 알고리즘2의 계산복잡성: 10000n+10000
        
        ⇒ 알고리즘1의 계산복잡도는 지수적으로, 알고리즘2는 선형적으로 증가하며, 2가 1보다 더 나은 알고리즘이라고 비교할 수 있게 된다.
        
        ![Untitled](/public/img/Algorithm/Asymptotic 712f8/Untitled.png)
        
    - 점근 표기법에는 대표적으로 대문자 O 표기법, 대문자 오메가(Ω) 표기법, 대문자 세타(Θ) 표기법, 소문자 o 표기법, 소문자 오메가(ω) 표기법 다섯 종류가 있다.
- Worst case running time은 알고리즘의 running time의 upper bound
- Average case는 Worst case만큼 bad

## Asymptotic upper bound

- Big-O notation
- f(n)이 n0보다 크거나 같은 모든 n에서 0이상이고, cg(n) 이하이면 O(g(n)) = f(n)
    
    ![Untitled](/public/img/Algorithm/Asymptotic 712f8/Lec01/Untitled 1.png)
    
- g(n)은 f(n)의 점근 상한 (asymptotic upper bound)
- 알고리즘 f(n)이 O(g(n))에 속한다면, f(n)의 계산복잡도는 최악의 경우라도 g(n)과 같거나 혹은 작다는 뜻
- ex> 3n+1=O(n)
    - n0≥1, c≥4 에서 0≤3n+1≤4n ⇒ 만족하는 c가 존재함!
    - 최대차항만 계수 없이 표기하면된다. 최악의 경우에도 4n보다 작거나 같다.

## Asymptotic lower bound

- Big-Ω notation
- 알고리즘 f(n)의 계산복잡도가 Ω(g(n))에 속한다면, f(n)의 계산복잡도는 최선의 경우를 상정하더라도 g(n)과 같거나 혹은 크다는 뜻
- 최선의 경우에도 cg(n)보다 크거나 같다.
    
    ![Untitled](/public/img/Algorithm/Asymptotic 712f8/Lec01/Untitled 2.png)
    

## Asymptotically tight bound

- 점근적 상한과 하한의 교집합
- Big Θ-notation
    
    ![Untitled](/public/img/Algorithm/Asymptotic 712f8/Lec01/Untitled 3.png)
    
- 내가 만든 알고리즘 f(n)이 아무리 나쁘거나 좋더라도 그 계산복잡도는 g(n)의 범위 내에 있다는 뜻
- ex> (n^2)/2-2n = Θ(n^2)
    - 8(n0) 이상인 모든 n에 대하여 0 ≤ c1n^2 ≤ (n^2)/2−2n ≤ c2n^2을 만족하는 c1,c2가 존재하기 때문

![Untitled](/public/img/Algorithm/Asymptotic 712f8/Lec01/Untitled 4.png)

출처:[https://ratsgo.github.io/data structure&algorithm/2017/09/13/asymptotic/](https://ratsgo.github.io/data%20structure&algorithm/2017/09/13/asymptotic/)