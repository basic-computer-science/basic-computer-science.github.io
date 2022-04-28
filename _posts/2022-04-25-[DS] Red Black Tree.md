---
layout: post
title:  "[Data Structure] Red Black Tree"
date:   2022-04-25 18:00:00 +0900
categories: DataStructure
---

# Red Black Tree

> **레드 블랙 트리**
> 

---

레드블랙 트리는 **자가균형이진탐색 트리**(self-balancing binary search tree)로써, 대표적으로 연관배열(associative array) 등을 구현하는데 쓰이는 자료구조이다. 레드-블랙 트리는 복잡한 자료구조이지만, 실 사용에서 효율적이고, 최악의 경우에도 상당히 우수한 실행 시간을 보인다. 트리에 n개의 원소가 있을때 Θ(log n)의 시간복잡도로 삽입, 삭제, 검색을 할 수 있다.

![Untitled](https://user-images.githubusercontent.com/100582309/165683693-9aaf8f13-de47-4380-8401-f1808d8cf862.png)


- 모든 노드가 빨간색 혹은 검은색인 이진 탐색 트리
- 모든 속성은 **Extended Binary Search Tree***를 기반으로 한다
    
    : 각 Null 포인터는 **외부 노드**(**External node**)로 바뀜
    
- **Black child**(외부 노드 포함)에 대한 포인터는 **검은색**이고,
    
    **Red child**에 대한 포인터는 **빨간색**이다.
    
<Br>

## * Extended Binary Tree
> **확장 이진트리**
> 
---
![Untitled 1](https://user-images.githubusercontent.com/100582309/165683694-f79ee8d5-20ae-4a5f-8cca-9e1d600a0ca9.png)

💡 **외부 노드** : 처음에는 다소 어색해 보일 수도 있지만 알고리즘에서 리프 노드가 개입될 때 특수 처리를 하지 않아도 되는 편리함이 있다.
 {: .notice}

<br><br>

## **Colored Node의 특징**

---

1. **root node** 는 **검은색**이다.
2. **모든 external node(=NIL)는** **검은색**이다.
3. **No double Red** = 루트 - 외부 노드 경로에 연속된 빨간색 노드가 없음
4. **모든 리프 노드에서 Black Depth는 같다** 
    
    : 모든 루트 - 외부 노드 경로에 동일한 수의 검은색 노드가 있음
    
<br>

## **삽입**

---

> **규칙**
> 
> 1. **이진탐색트리에서 하는 일반적인 삽입**을 한다.
> 2. 레드-블랙 트리에 새로운 노드를 삽입할 때 **새로운 노드**는 항상 **빨간색**으로 삽입한다.
> 3. **레드-블랙 트리의 조건**에 맞추기 위해 **트리를 수정**한다.

<br>

### **`case 1` : 새로운 노드의 부모노드가 블랙일 때**

![Untitled 2](https://user-images.githubusercontent.com/100582309/165683695-385d5290-577e-4348-9b9b-d8b8bc1939eb.png)

![Untitled 3](https://user-images.githubusercontent.com/100582309/165683662-0c173c0e-6b23-4608-ae73-259f3917dfba.png)

![Untitled 4](https://user-images.githubusercontent.com/100582309/165683668-7ed42f55-8be8-4b51-b1cd-64d127a83c0d.png)

<br>

### **`case 2` : 새로운 노드의 부모가 레드일 때**

이 때는 **빨간색 노드가 연속으로 2번 나타나는 현상이 일어난다(Double Red).** 레드 블랙 트리는 이러한 Double Red 문제를 해결하기 위해 2가지 전략을 사용한다.


| ![Double Red 해결법](https://blog.kakaocdn.net/dn/bYG3yV/btrpoxGRp6g/fBAd1VvrqdWy6QRRSKTX3k/img.png) | 
|:--:| 
|Double Red 해결법|

앞으로 새로 삽입할 노드를 **N**(New), 부모 노드를 **P**(Parent), 조상 노드를 **G**(Grand Parent), 삼촌 노드를 **U**(Uncle)라고 하자. 즉, 삼촌 노드는 말 그대로 부모의 형제라고 생각하면 된다.

Double Red가 발생했을 때

- **삼촌 노드(U)**가 검은색이라면 -> **Restructuring**을 수행하면 된다.
- **삼촌 노드(U)**가 빨간색이라면 -> **Recoloring**을 수행하면 된다.
    
<br>    

## **Restructuring**

Restructuring은 다음 과정을 거친다.

1. 새로운 노드(N), 부모 노드(P), 조상 노드(G)를 오름차순으로 정렬한다. 

    | ![Restructuring 1단계](https://blog.kakaocdn.net/dn/qszUV/btrpdq9Hhyf/HgbBFX55xi67E3JkjPpx7K/img.png) | 
    |:--:| 
    |Restructuring 1단계|

2. 그러면 3이 중간값이 된다. 따라서 중간값인 3을 부모 노드로 바꾸고 나머지 2와 5를 자식 노드로 바꾼다.
    
    💡 당연히 원래 5의 자식 노드였던 7(U)은 딸려가게 된다. 헷갈리지 말자!
    {: .notice}
    
    | ![Restructuring 2단계](https://blog.kakaocdn.net/dn/cAUx5F/btrppAcjYXR/czbUJZ1GU22ufSGewtyYQ1/img.png) | 
    |:--:| 
    |Restructuring 2단계|

3. 마지막으로 새롭게 부모가 된 3을 검은색으로 바꿔주고 나머지 두 자식인 2, 5의 색을 빨간색으로 바꿔주면 Double Red 문제가 해결된다.
    
    💡 여기서 많이들 헷갈리는 게, 완성된 트리가 규칙 2(모든 외부 노드는 검은색)을 만족하지 않는 것처럼 보일 수 있다. 값이 2인 노드는 자식 노드 NIL 2개를 가지고 있고 그 NIL들이 검은색이라고 생각하면 된다.
    {: .notice}

    | ![Restructuring 3, 4단계](https://blog.kakaocdn.net/dn/MASjd/btrpq6WhqWJ/6Vg1qcMarQEqQDk1oKGi51/img.png) | 
    |:--:| 
    |Restructuring 3, 4단계|
    
<br>    

## **Recoloring**

Recoloring은 다음과 같은 과정을 거친다.

1. 새로운 노드(N)의 부모(P)와 삼촌(U)을 검은색으로 바꾸고 조상(G)을 빨간색으로 바꾼다.
   
    | ![Recoloring 1단계](https://blog.kakaocdn.net/dn/dWUP6r/btrpjv39bFN/H0wFIBAK3bV3cDrG5PwH4K/img.png) | 
    |:--:| 
    |Recoloring 1단계|

2. 루트 노드는 검은색이라는 조건을 지켜야 하므로, 루트 노드를 검은색으로 바꾼다. 이렇게 하면 모든 조건이 지켜지면서 Double Red 문제가 해결된다.

    | ![Recoloring 1, 2단계](https://blog.kakaocdn.net/dn/nkkuw/btrpjvpzukc/ZprBjMgiPVQzBJxPgaZiU1/img.png) | 
    |:--:| 
    |Recoloring 1, 2단계|


💡 검은색 노드는 2번 나와도 되냐고 묻는다면 Yes이다. 빨간색 노드가 2번 나오면 안 되는 것이다. 헷갈리지 말자!!
{: .notice}

Recoloring은 간단해 보이지만 문제는 조상 노드(G)가 루트 노드가 아니면서, 조상 노드(G)가 또다시 Double Red 문제가 발생하는 경우이다.

![https://blog.kakaocdn.net/dn/vBBus/btrpjwouNiw/cBnlbiBxKyKUb8XRBvf4D1/img.png](https://blog.kakaocdn.net/dn/vBBus/btrpjwouNiw/cBnlbiBxKyKUb8XRBvf4D1/img.png)

위와 같은 상황을 가정하자. 왼쪽 트리에서 Recoloring을 진행하면 오른쪽 트리가 된다.

이때 조상 노드(G)가 또다시 Double Red가 발생하게 된다.

![https://blog.kakaocdn.net/dn/bawsxN/btrpoyMAmYw/3KxRGRUuFwJU5KTsmkwNJ0/img.png](https://blog.kakaocdn.net/dn/bawsxN/btrpoyMAmYw/3KxRGRUuFwJU5KTsmkwNJ0/img.png)

Double Red 문제가 발생한 "값이 5인 노드"를 기준으로 다시 한번 살펴보자.

해당 노드의 삼촌(U)이 빨간색이므로 다시 Recoloring을 진행해주면 Double Red 문제를 해결할 수 있다!

만약 해당 노드의 삼촌(U)가 검은색이었다면 Restructuring을 진행해주면 된다.

<br>    

## **삭제**

---

조금 더 까다로운 삭제를 알아보겠다.

### **case default : 블랙 노드를 삭제할 경우, 자식을 블랙으로 칠한다**

**💡 case default** : 기능이 입력되면 무조건 실행되는 케이스
만약 삭제하려는 노드가 레드면 그 노드만 삭제해주면 되고, 블랙이더라도 child node가 레드라면 해당 노드를 삭제해주고 child의 색깔을 블랙으로 바꿔주면 아무 문제가 없다.
{: .notice}

![Untitled 11](https://user-images.githubusercontent.com/100582309/165683681-a5c0c6e6-8527-4143-9b05-aae0e75b3871.png)

<br>

### ✏ problem case : 자식이 블랙인 블랙 노드 삭제 → 이중 흑색 노드 발생

문제가 되는 경우는 삭제하려는 노드와 그 자식 노드의 색깔이 모두 Black일 경우이다. 루트에서 외부 노드까지의 블랙 노드의 개수는 모두 일정해야 하기 때문이다. 여기서 블랙 노드를 하나 삭제해버린다면 그런 특성이 깨지기 때문에 문제가 생긴다.

![Untitled 12](https://user-images.githubusercontent.com/100582309/165683685-b54f0ec8-e585-43df-9983-6e782ca4f165.png)

이러한 경우 이중 흑색 노드가 생겨나며, 따라서 이를 해결하기 위해 삭제의 경우에도 몇 가지 경우를 나누어 생각해볼 수 있다.

<br>

### 이중 흑색 노드의 처리

---

**이중 흑색 노드가 부모 노드의 왼쪽 자식일 경우**로 생각해보자. (오른쪽 자식일 경우는 구현할 때 방향을 모두 반대로 해주면 된다.)                       

| ![Untitled 13](https://user-images.githubusercontent.com/100582309/165683686-5b366307-3907-48fc-81fb-d3d2b962df83.png) | 
|:--:| 
|이중 흑색의 개념|

위 그림과 같이 이중 흑색을 만든 다음 이 이중 흑색을 원래대로 되돌리는 후처리를 하면 삭제가 완료된다.

> 총 4가지의 경우가 있다.
> 
> 1. 형제가 레드인 경우
> 2. 형제는 블랙이고
>     1. 형제의 양쪽 자식이 모두 블랙인 경우
>     2. 형제의 왼쪽 자식은 레드, 오른쪽 자식은 블랙인 경우
>     3. 형제의 오른쪽 자식이 레드인 경우

<br>

**✏ Case 1 : 형제가 레드인 경우**

형제를 블랙, 부모를 레드로 칠한다. 부모 노드를 기준으로 좌회전한다.

![Untitled 14](https://user-images.githubusercontent.com/100582309/165683688-d284a0c1-0c48-4ee7-92a9-6d40c089df81.png)


이 경우에도 여전히 이중 흑색 노드이며, 달라진 점은 형제가 레드가 아닌 블랙으로 바뀌었다는 점이다. 그럼 이제 case 2로 넘어간다.

<br>

**✏ Case 2-a : 형제가 블랙이고 형제의 양쪽 자식 모두 블랙인 경우**

형제 노드만 레드로 칠한 다음, 이중 흑색 노드가 갖고 있는 블랙 중 하나를 부모 노드에게 넘겨주면 된다. 그 후 블랙 노드의 처리는 다시 4가지의 경우로 나누어 후처리를 진행한다.

<br>

**✏ Case 2-b : 형제가 블랙이고 형제의 왼쪽 자식은 레드, 오른쪽 자식은 블랙인 경우**

형제 노드를 레드로 칠하고 왼쪽 자식을 블랙으로 칠한 다음, 형제 노드를 기준으로 우회전합니다. 그렇다면 2-b의 경우가 2-c의 경우로 바뀌게 된다.

<br>

**✏ Case 2-c : 형제가 블랙이고 형제의 오른쪽 자식이 레드인 경우**

이중 흑색 노드의 부모 노드가 갖고 있는 색을 형제 노드에 칠한다. 그다음 부모 노드와 형제 노드의 오른쪽 자식 노드를 블랙으로 칠하고 부모 노드를 기준으로 좌회전 후 루트 노드에게 블랙으로 넘기고 이중 흑색을 없애주면 상황은 끝이 나게 된다.

![Untitled 15](https://user-images.githubusercontent.com/100582309/165683691-858b0d07-ca85-4205-a498-78e721c5e530.png)

<br>

### 참고 자료

- [덕's IT Story 블로그] [https://itstory.tk/entry/레드블랙-트리Red-black-tree](https://itstory.tk/entry/%EB%A0%88%EB%93%9C%EB%B8%94%EB%9E%99-%ED%8A%B8%EB%A6%ACRed-black-tree)
- [코드 연구소] [https://code-lab1.tistory.com/62](https://code-lab1.tistory.com/62)
- [https://hyunipad.tistory.com/15](https://hyunipad.tistory.com/15)
- [https://slidesplayer.org/slide/16283436/](https://slidesplayer.org/slide/16283436/)