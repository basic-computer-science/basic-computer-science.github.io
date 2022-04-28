---
layout: post
title:  "[Data Structure] B-tree, B+tree"
date:   2022-04-27 10:00:00 +0900
categories: DataStructure
---

## DB에서는 왜 이진 탐색 트리가 아닌 다원 탐색 트리, 그 중 Btree를 많이 사용하는가?**
---

우선, 이진 탐색 트리를 사용하면 n개의 원소에서 특정 값을 검색하는데에 O(log n)이므로 매우 빠르다. 하지만, **현대 컴퓨터에서는 산술, 논리 연산보다는 메모리 접근 연산이 훨씬 많은 시간을 소모한다.** 이게 무슨 뜻인지 살펴보자.

<br>

**이진 탐색 트리**는 우선 '내부 검색'에 적절한 구조라고 할 수 있다. 데이터를 메인 메모리에만 올려두고 사용할 때에는 효율적인 구조라는 뜻이다. 그러나 데이터의 양이 방대해지면, 모든 노드들을 메인 메모리(RAM)에 올려서 사용할수가 없으니 결국 디스크(보조 기억 장치)에 탐색 트리를 두고 검색을 해야 한다. 즉, cpu가 디스크에 접근을 해서 작업해야 한다는 뜻이다.

<br>

운영체제에서 배웠다시피, **디스크 접근은 메인 메모리 접근에 비해 훨씬 느리다.** 즉, 그 무엇보다도 **__디스크 접근을 최소화__해야 빠른 시간에 검색을 마칠 수 있는데,** 이진 트리는 트리가 높아지고 불균형상태가 됨에 따라 성능이 크게 좌우된다는 특징이 있기 때문에, 대용량 데이터 처리에 문제가 생길 수 있다. 

<br>

**다원 탐색 트리**는 '외부 검색'에 매우 효율적인 구조이다. 기존의 BST에서 데이터 접근 과정에서 오는 낭비를 최소화 하고자 한 노드의 데이터를 여러개를 저장할 수 있도록 degree를 늘인 형태이기 때문이다.

<br>

이때 이, 다원 탐색 트리에서도 마찬가지로 level을 최소화하면 색인(index) 구조를 통해 탐색 성능을 더욱 향상시킬 수 있는데 이를 구현한 것이 **Btree**이다.

B트리의 노드를 한 개의 블록 크기와 일치시키고, 넣을 수 있을 만큼 key와 부모-자식 포인터들을 넣는다. 이렇게 하면 한 번의 디스크 접근에서 최대 효율로 데이터들을 검색 트리에서 읽어올 수 있으므로 디스크 접근 횟수를 줄일 수 있다.

이러한 이유로 데이터베이스 시스템은 대부분 M-way search tree이자 Balanced tree인 B-tree 혹은 B+tree를 사용한다.

<br>

📌 **내부 검색과 외부 검색**
<br>
- 내부 검색 : 메모리 안에 들어있는 데이터를 검색할 때
- 외부 검색 : 보조기억장치 안에 들어있는 데이터를 검색할 때
{: .notice}


📌 **Block (블록)**
<br>
   : 디스크 I/O의 기본 단위. 즉, 디스크 공간을 구분하는 단위이며 고정된 값이다. 소프트웨어에서는 page라고 부르기도 한다.
{: .notice}


<br><br>

# **M-way Search Tree**

> Multiway Search Tree, 다원 탐색 트리
> 

---

![Untitled](https://user-images.githubusercontent.com/100582309/165692065-97645313-0241-4ec4-a0c4-63b5bd0a9284.png)

- 다원 탐색 트리는 한 노드안에 최대 m-1개의 요소와 m개의 자식을 가질 수 있다.
    
    → 사실, 이진 탐색 트리는 m=2 인 다원 탐색 트리이다.
    
- 각 노드 안에서 자료들은 key에 의해 정렬된다.
- 트리의 높이를 h라 하면 트리의 작업 속도는 O(h)이다.
    
    ⇒ h(높이)를 낮춤으로써 작업 속도를 높일 수 있다. 그러기 위해서는 노드에 저장할 수 있는 요소의 수를 늘림으로써 트리의 높이를 줄일 수 있다.
    
| ![이진 탐색 트리 vs 다원 탐색 트리 (데이터 15개인 경우)](https://blog.kakaocdn.net/dn/blVEjY/btq3TrBChbE/4OnAvKKvzLxNuaLuQ7Org0/img.png) | 
|:--:| 
|이진 탐색 트리 vs 다원 탐색 트리 (데이터 15개인 경우)|                                    


- `장점` : 다원 탐색 트리는 이진 검색 트리보다 많은 요소를 저장할 수 있어 낮은 높이를 유지할 수 있다.
- `단점` : 다원 탐색 트리는 스스로 균형을 유지하지 못하기 때문에 불균형이 발생할 수 있다는 단점이 있다. 이럴 경우 검색 성능이 떨어지게 된다.


이런 다윈 탐색 트리의 단점을 보완하기 위해 스스로 균형을 유지하는 기능이 필요하다. 이를 갖춘 트리로는 대표적으로 B-tree가 있다.

<br>

# **B-Tree**

---

💡 **B-Tree는 무슨 줄임말인가?**
<br>
B-트리의 창시자인 루돌프 바이어는 'B'가 무엇을 의미하는지 따로 언급하지는 않았다. 가장 가능성 있는 대답은 리프 노드를 같은 높이에서 유지시켜주므로 균형잡혀있다(balanced)는 뜻에서의 'B'라는 것이다. '바이어(Bayer)'의 'B'를 나타낸다는 의견도, 혹은 그가 일했던 보잉 과학 연구소(Boeing Scientific Research Labs)에서의 'B'를 나타낸다는 의견도 있다.
{: .notice}

💡 **Btree는 정렬된 순서를 보장하고, 멀티레벨 인덱싱을 통한 빠른 검색을 할 수 있기 때문에 DB에 사용한다.**
{: .notice}

<br>

B트리는 이진트리와 다르게 하나의 노드에 많은 수의 정보를 가지고 있을 수 있다. **최대 *m*개의 자식**을 가질 수 있는 B트리를 ***m*차 B트리**라고 하며 다음과 같은 특징을 같습니다.

- **모든 리프 노드**는 **같은 레벨**에 있다 (**perfectly balanced**)
- **루트 노드**는 **리프 노드**이거나 **최소 2개의 자식 노드**가 있다.
- 루트 노드와 리프 노드가 아닌 **노드**는 ***m*/2개**부터 **최대 *m*개**까지의 **자식**을 가질 수 있다.
- 노드에는 **최대 *m*−1개 부터 [*m*/2]−1개의 key**가 포함될 수 있다.
- 노드의 **key가 *x*개**라면 **자식의 수는 *x*+1개**이다.
- **최소차수**는 **자식수의 하한값**을 의미하며, **최소차수가 *t***라고 하면 ***m=2t−1***을 만족한다.
    
    (최소차수 *t*가 2라면 3차 B트리이며, key의 하한은 1개입니다.)
    
     → m=3일 때, B 트리의 모든 내부 노드의 차수는 2 또는 3 (**2-3 tree**)
    
    m=4일 때, B 트리의 모든 내부 노드의 차수는 2 또는 3 또는 4 (**2-3-4 tree**)
    
    ![Untitled 1](https://user-images.githubusercontent.com/100582309/165692066-7897d8d0-a824-4fa6-ade1-a5a2f70cb0f7.png)
    
    | ![Untitled 2](https://user-images.githubusercontent.com/100582309/165692068-86421515-fb16-4108-b5bb-4070fba50464.png) | 
    |:--:| 
    |차수가 3인 B트리|

    → 파란색 부분은 **각 노드의 key**, 빨간색 부분은 **자식 노드들을 가르키는 포인터**

    → key들은 노드 안에서 항상 정렬된 값을 가짐

    ⇒ BST처럼 각 key들의 왼쪽 자식들은 항상 key보다 작은 값을, 오른쪽은 큰 값을 가진다.

<br><br>

# **key 검색과정**

---

루트노드에서 시작하여 **하향식으로 검색**을 수행한다. 검색하고자 하는 key를 *k*라고 하였을 때 검색 과정이다.

1. 루트 노드에서 시작하여 **key들을 순회**하면서 검사한다.
    
    1-1. 만일 *k*와 같은 key를 찾았다면 검색을 종료한다.
    
    1-2. 검색하는 값과 key들의 **대소관계를 비교**한다. 어떠한 key들 사이에 *k*가 들어간다면 해당 key들 사이의 자식노드로로 내려간다.
    
2. 해당 과정을 **리프노드에 도달할 때까지 반복**한다. 만일 리프노드에도 *k*와 같은 key가 없다면 검색을 실패한다.

![Untitled 3](https://user-images.githubusercontent.com/100582309/165692070-6d18a9a0-cd30-44bb-985c-13e30cb35c55.png)

![Untitled 4](https://user-images.githubusercontent.com/100582309/165692073-0867fd09-cb32-4f79-ac56-75fd912c16b2.png)

<br>

# **key 삽입과정**

---

key를 삽입하기 위해서는 **1. 요소 삽입에 적절한 리프 노드를 검색**하고, **2. 필요한 경우 노드를 분할**해야 한다. 리프노드 검색은 하향식이지만 노드 분할의 과정은 **상향식**으로 이루어진다고 볼 수 있다. 삽입하고자 하는 값을 *k*로 하였을 때 삽입 과정이다.

1. 트리가 비어있으면 루트 노드를 할당하고 *k*를 삽입한다. 만일 루트노드가 가득 찼다면, **노드를 분할**하고 리프노드가 생성된다.
2. 이후부터는 삽입하기에 적절한 리프노드를 찾아 *k*를 삽입한다. 삽입위치는 노드의 key값과 *k*값을 **검색 연산과 동일한 방법으로** 비교하면서 찾는다.

<br>

이후 두가지 케이스로 나뉘게 된다.

### ✏ Case 1. 분할이 일어나지 않는 경우

> 리프노드가 가득차지 않았다면, **오름차순으로 *k*를 삽입**한다.
> 

---

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2F95b4c5c3-c267-4423-865e-778a68ad4a50%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%BD%EC%9E%85%201-1.png](https://velog.velcdn.com/images%2Femplam27%2Fpost%2F95b4c5c3-c267-4423-865e-778a68ad4a50%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%BD%EC%9E%85%201-1.png)

<br>

### ✏ Case 2. 분할이 일어나는 경우

> 만일 리프노드에 key노드가 가득 찬 경우, **노드를 분할**해야 합니다.
> 

---

1. 오름차순으로 요소를 삽입한다. 노드가 담을 수 있는 **최대 key 개수를 초과**하게 된다.
2. **중앙값에서 분할을 수행한다**. 중앙값은 부모 노드로 **병합하거나 새로 생성된**다. 왼쪽 키들은 왼쪽 자식으로, 오른쪽 키들은 오른쪽 자식으로 **분할된**다.
3. 부모 노드를 검사해서 또 다시 가득 찼다면, 다시 부모노드에서 위 과정을 반복한다.

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2F4b5003e5-55de-441c-a3ee-15e4db7a2abd%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%BD%EC%9E%85%202-1.png](https://velog.velcdn.com/images%2Femplam27%2Fpost%2F4b5003e5-55de-441c-a3ee-15e4db7a2abd%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%BD%EC%9E%85%202-1.png)

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2F13ab96a4-04cc-42a7-bb01-eac1276bdf67%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%BD%EC%9E%85%202-2.png](https://velog.velcdn.com/images%2Femplam27%2Fpost%2F13ab96a4-04cc-42a7-bb01-eac1276bdf67%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%BD%EC%9E%85%202-2.png)

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fd99cdbc8-c5b4-4667-be7d-2589adca45e8%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%BD%EC%9E%85%202-3.png](https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fd99cdbc8-c5b4-4667-be7d-2589adca45e8%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%BD%EC%9E%85%202-3.png)

<br><br>

# **key 삭제과정**

---

요소를 삭제하기 위해선 **1. 삭제할 키가 있는 노드 검색, 2. 키 삭제, 3. 필요한 경우, 트리 균형 조정**을 해야 한다.

삭제 과정을 이해하기 위해서 몇가지 용어를 정의하였다.

- `inorder predecessor`(중위선행자) : 노드의 **왼쪽 자손**에서 가장 **큰 key**
- `inorder successor`(중위후행자) : 노드의 **오른쪽 자손**에서 가장 **작은 key**
- 부모 key : 부모노드의 key들 중 왼쪽 자식으로 본인 노드를 가지고 있는 key값이다. 단, 마지막 자식노드의 경우에는 부모의 마지막 key이다.

### ✏ Case 1. 삭제할 key *k*가 리프에 있는 경우
<br>

**Case 1.1) 현재 노드의 key 개수가 최소 key 개수보다 크다면**

➡ 다른 노드들에 영향 없이 해당 *k*를 **단순 삭제**합니다.

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fe0e2045e-33a2-439f-a781-f6f9af8d0b66%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%AD%EC%A0%9C%201-1.png](https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fe0e2045e-33a2-439f-a781-f6f9af8d0b66%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%AD%EC%A0%9C%201-1.png)

<br>

**Case 1.2) 왼쪽 또는 오른쪽 형제 노드의 key가 최소 key 개수 이상이라면**

1. 부모 key 값으로 *k*를 대체합니다.
2. 최소key 개수 이상의 key를 가진 형제 노드가 왼쪽 형제라면 가장 큰 값을, 오른쪽 형제라면 가장 작은 값을 부모key로 대체합니다.

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2F8e7b0f78-ae26-48df-8925-47171c588c48%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%AD%EC%A0%9C%201-2.png](https://velog.velcdn.com/images%2Femplam27%2Fpost%2F8e7b0f78-ae26-48df-8925-47171c588c48%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%AD%EC%A0%9C%201-2.png)

**Case 1.3) 왼쪽, 오른쪽 형제 노드의 key가 최소 key 개수이고, 부모노드의 key가 최소개수 이상이면**

1. *k*를 삭제한 후, 부모key를 형제 노드와 병합합니다.
2. 부모노드의 key개수를 하나 줄이고, 자식 수 역시 하나를 줄여 **B-Tree를 유지**합니다.

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fdde5e5ae-892c-4d1c-9299-4710023f7531%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%AD%EC%A0%9C%201-3.png](https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fdde5e5ae-892c-4d1c-9299-4710023f7531%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%AD%EC%A0%9C%201-3.png)

**Case 1.4) 자신과 형제, 부모 노드의 key 개수가 모두 최소 key 개수라면**

부모노드를 루트노드로 한 **부분 트리의 높이가 줄어드는 경우**이기 때문에 재구조화의 과정이 일어납니다. case3의 2번 과정으로 이동합니다.

<br>

### 💡 Case 2. 삭제할 key *k*가 내부 노드에 있고, 노드나 자식에 key가 최소 key수보다 많을 경우

1. 현재 노드의 `inorder predecessor` 또는 `inorder successor`와 *k*의 자리를 바꿉니다.
2. 리프노드의 *k*를 삭제하게 되면, 리프노드가 삭제 되었을 때의 조건으로 변합니다. 삭제한 리프노드에 대해서 case 1 조건으로 이동합니다.

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2F6d4a5d37-1633-45a1-8225-c6e558031865%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%AD%EC%A0%9C%202.png](https://velog.velcdn.com/images%2Femplam27%2Fpost%2F6d4a5d37-1633-45a1-8225-c6e558031865%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%AD%EC%A0%9C%202.png)

<br>

### 💡 Case 3. 삭제할 key *k*가 내부 노드에 있고, 노드에 key 개수가 최소 key개수만큼, 노드의 자식 key 개수도 모두 최소 key 개수인 경우

삭제할 key *k*가 있는 노드도 최소, 자식노드들도 최소의 key 개수를 가지므로, *k*를 삭제하면 트리의 높이가 줄어들어 **재구조화**가 일어나는 케이스입니다. 재구조화의 과정은 다음과 같습니다.

1. *k*를 삭제하고, *k*의 양쪽 자식을 병합하여 하나의 노드로 만듭니다.
2. *k*의 부모key를 인접한 형제 노드에 붙입니다. 이후, 이전에 병합했던 노드를 자식노드로 설정합니다.
3. 해당 과정을 수행하였을 때 부모노드의 개수가 에 따라 이후 수행 과정이 달라집니다.3-1. 만일 새로 구성된 인접 형제노드의 key가 최대 key 개수를 넘어갔다면, 삽입 연산의 **노드 분할 과정을 수행**합니다.3-2. 만일 인접 형제노드가 새로 구성되더라도 원래 *k*의 부모 노드가 최소 key의 개수보다 작아진다면, **부모 노드에 대하여 2번 과정부터 다시 수행**합니다.

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2F84dbc50f-fff4-4207-8e27-a34b9043f798%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%AD%EC%A0%9C%203-1.png](https://velog.velcdn.com/images%2Femplam27%2Fpost%2F84dbc50f-fff4-4207-8e27-a34b9043f798%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%AD%EC%A0%9C%203-1.png)

<br>

⇒ 새로운 트리에서 예시

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fe2f82f30-2f9c-4177-a908-1b5333f8e9d6%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%AD%EC%A0%9C%203-2.png](https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fe2f82f30-2f9c-4177-a908-1b5333f8e9d6%2FB%ED%8A%B8%EB%A6%AC%20%EC%82%AD%EC%A0%9C%203-2.png)

<br><br>

# **DB 시스템에서 B-tree의 활용**

---

- 디스크의 접근 횟수는 O(log**ₘ**n)
- 각 디스크 액세스는 branch 방향을 결정하기 위해 O(logm) 오버헤드가 필요하지만, 이는 하드 디스크 액세스 없이 메인 메모리에서 수행되므로 무시할 수 있다.
- m은 가능한 한 크게 결정할 수 있지만, 내부 노드가 하나의 디스크 블록에 들어갈 수 있을 만큼은 작아야 한다.
- 이 때, m은 보통 32에서 256 사이이다.
- 종종 하나 또는 두 레벨의 내부 노드가 메인 메모리에 상주한다.

<br>
<br>

# **B+tree**

---

B+tree는 B-tree의 변형 구조로, **index 부분**과 **리프 노드로 구성된 순차 데이터 부분**으로 이루어진다. 즉, B-tree의 그림에서는 편의상 data를 생략해서 그렸지만 각 key값이 data를 가지고 있었고, B+tree의 경우에는 인덱스 부분의 key 값은 리프 노드에 있는 key 값을 직접 찾아가는데 사용한다고 생각하면 된다.

동작 방식의 다른점이라고 하면 **리프 노드가 연결리스트의 형태**를 띄어 **선형 검색이 가능하다**는 점이 있다. 이러한 특징점 때문에 **굉장히 작은 시간복잡도**에 **검색**을 수행할 수 있다. 

이런 장점들 때문에 모든 relational database system에서 B+ Tree를 지원하고 있으며, 거의 모든 file system에서도 B+ Tree를 사용한다.

💡 실제 **DB의 인덱싱은 B+트리**로 이루어져있다고 한다. 다음 그림은 인덱싱을 나타낸 것이다. 인덱싱은 어떠한 자료를 찾는데 key값을 이용해 효과적으로 찾을 수 있는 기능이다.
![Untitled 5](https://user-images.githubusercontent.com/100582309/165692074-35ea7971-894e-49fd-88aa-ab9dbd3fb7b9.png)
다음과 같은 인덱싱을 **B+트리**로 나타내면 아래 그림과 같습니다.
![Untitled 6](https://user-images.githubusercontent.com/100582309/165692057-b3f1e320-73a7-4a45-8b24-db2288328d77.png)
{: .notice}

<br>

## B+tree의 특징

---

![Untitled 7](https://user-images.githubusercontent.com/100582309/165692061-83de8de1-6b54-4a80-8b13-2fbad25e3af2.png)


### B+tree가 B-tree와 다른 점

1. **모든 key, data가 리프노드에 모여있다.**
    
    B트리는 리프노드가 아닌 각자 key마다 data를 가진다면, B+트리는 리프 노드에 모든 data를 가진다.
    
2. **모든 리프노드가 연결리스트의 형태를 띄고 있다.**
    
    B트리는 옆에있는 리프노드를 검사할 때, 다시 루트노드부터 검사해야 한다면, B+트리는 리프노드에서 선형검사를 수행할 수 있어 시간복잡도가 굉장히 줄어든다.
    
3. **데이터의 빠른 접근을 위한 인덱스 역할만 하는 비단말 노드(Non-Leaf)가 있다.**
    
    기존의 B-Tree와 데이터의 연결리스트로 구현된 색인구조. 데이터의 빠른 접근을 위한 인덱스 역할만 한다.
    
4. key 값을 기준으로 **왼쪽 pointer**는 key 값보다 **작은** key 값의 노드를,
    
    key 값을 기준으로 **오른쪽 pointer**는 key 값보다 **크거나 같은** key 값의 노드를 가리킨다.
    

💬 **B+tree가 B-tree와 같은 점 (*m*차 B+트리에 대하여)**
- 노드는 최대 *m*개 부터 *m*/2개 까지의 자식을 가질 수 있다.
- 노드에는 최대 *m*−1개 부터 [*m*/2]−1개의 키가 포함될 수 있다.
- 노드의 키가 *x*개라면 자식의 수는 *x*+1개이다.
- 최소차수는 자식수의 하한값을 의미하며, 최소차수가 *t*면 *m*=2*t*−1을 만족한다.
    (최소차수 *t*가 2라면 3차 B트리이며, key의 하한은 1개이다.)
{: .notice}

<br>

**`장점`** 

- **블록 사이즈를 더 많이 이용할 수 있음**
    
    key 값에 대한 하드디스크 액세스 주소가 없기 때문이다. 
    
    리프 노드를 제외하고 데이터를 담아두지 않기 때문에 메모리를 더 확보함으로써 더 많은 key들을 수용할 수 있다. 하나의 노드에 더 많은 key들을 담을 수 있기에 트리의 높이는 더 낮아진다.(cache hit를 높일 수 있음)
    
- **리프 노드끼리 연결 리스트로 연결되어 있어서 범위 탐색에 매우 유리**
    
     ex : Full scan시 선형탐색 1번이면 된다.
    
<br>

**`단점`**

B-tree의 경우 최상 케이스에서는 루트에서 끝날 수 있지만, B+tree는 무조건 리프 노드까지 내려가봐야 함

<br><br>

# key 삽입과정

---

모든 설명의 과정은 리프노드가 삽입 또는 삭제 될 때 트리의 key가 변경되는 경우로 진행하겠습니다. **검색과정은 B트리와 동일**하기 때문에 생략하고 삽입의 과정부터 시작하겠습니다. 삽입의 과정도 B트리와 매우 유사하지만 리프노드에서 최대 key개수를 초과할 때가 다릅니다.

<Br>

### 💡Case 1. 분할이 일어나지 않고, 삽입 위치가 리프노드의 가장 앞 key 자리가 아닌 경우

B트리와 똑같은 삽입 과정을 수행합니다.

<br>

### 💡Case 2. 분할이 일어나지 않고, 삽입 위치가 리프노드의 가장 앞 key 자리인 경우

삽입 후 **부모 key를 삽입된 key로 갱신**하고, data를 넣어줍니다.

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fd69592b9-c14e-4cc5-ad7b-4ea36120035c%2F%EC%82%BD%EC%9E%85%202-1.jpg](https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fd69592b9-c14e-4cc5-ad7b-4ea36120035c%2F%EC%82%BD%EC%9E%85%202-1.jpg)

<Br>

### 💡Case 3. 분할이 일어나는 삽입과정

1. 분할이 일어나는 노드가 **리프노드가 아니라면** 기존 B트리와 똑같이 분할을 진행합니다. 중간 key를 부모 key로 올리고, 분할한 두개의 노드를 왼쪽, 오른쪽 자식으로 설정합니다.
2. 분할이 일어나는 노드가 **리프노드라면** 중간 key를 부모 key로 올리지만, 오른쪽 노드에 **중간 key를 포함하여 분할**합니다. 또한 리프노드는 연결리스트이기 때문에 **왼쪽 자식노드와 오른쪽 자식 노드를 이어줘 연결리스트 형태를 유지합니다.** 해당 부분이 B트리의 분할과 다른 점입니다.

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2F251a8b34-b943-41c2-9391-3fea1d9a5b29%2F%EC%82%BD%EC%9E%85%203-1.jpg](https://velog.velcdn.com/images%2Femplam27%2Fpost%2F251a8b34-b943-41c2-9391-3fea1d9a5b29%2F%EC%82%BD%EC%9E%85%203-1.jpg)

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fd4b9cb28-1b18-4ac1-802c-020dee95ffb9%2F%EC%82%BD%EC%9E%85%203-2.jpg](https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fd4b9cb28-1b18-4ac1-802c-020dee95ffb9%2F%EC%82%BD%EC%9E%85%203-2.jpg)

<Br>

# key 삭제과정

---

삭제과정 역시 기존 B트리와 유사합니다만 삭제할 key *k*는 무조건 리프노드에 존재하는 점, *k*를 삭제하기 위해 검색하는 과정에서 index에 존재할 수 있다는 점이 다릅니다. 해당 부분을 위주로 삭제 과정을 적어보겠습니다.

<Br>

### 💡 Case 1. 삭제할 key k가 index에 없고, 리프노드의 가장 처음 key가 k가 아닌경우

기존의 B트리 삭제과정과 동일합니다.

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fce1bbae2-3ade-4970-854d-e7ad9f526283%2F%EC%82%AD%EC%A0%9C%201.jpg](https://velog.velcdn.com/images%2Femplam27%2Fpost%2Fce1bbae2-3ade-4970-854d-e7ad9f526283%2F%EC%82%AD%EC%A0%9C%201.jpg)

<br>

### 💡 Case 2. 삭제할 key k가 리프노드의 가장 처음 key인 경우

❗ 삭제할 key *k*가 리프노드의 가장 처음 key인 경우에는 **항상 *k*가 index 내에 존재합니다.**
{: .notice}

1. 먼저 리프노드의 *k*를 삭제하는 과정을 수행합니다. key의 개수가 최소 key의 개수라면 B트리의 삭제 과정 중 형제노드의 key를 빌려오는 경우나 부모key와 병합하는 등 과정들은 **동일하게 수행**합니다. 단, **리프노드가 병합**할 때는 부모key와 오른쪽 자식의 처음 key가 동일하기 때문에 부모key를 가져오는 과정만 생략하고 **왼쪽 자식과 오른쪽 자식을 병합**만 하면 됩니다.
2. 리프노드의 k를 삭제한 후, index내의 *k*를 **`inorder successor`로 변경**합니다.

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2F25579855-5b0f-4cbb-a400-9fb5a5645d0d%2F%EC%82%AD%EC%A0%9C%202-1.jpg](https://velog.velcdn.com/images%2Femplam27%2Fpost%2F25579855-5b0f-4cbb-a400-9fb5a5645d0d%2F%EC%82%AD%EC%A0%9C%202-1.jpg)

![https://velog.velcdn.com/images%2Femplam27%2Fpost%2F1e256a60-912f-4369-8444-e74c535ecc3c%2F%EC%82%AD%EC%A0%9C%202-2.jpg](https://velog.velcdn.com/images%2Femplam27%2Fpost%2F1e256a60-912f-4369-8444-e74c535ecc3c%2F%EC%82%AD%EC%A0%9C%202-2.jpg)

<br><br>

# **B-tree vs B+tree**

---

| 구분 | B-tree | B+tree |
| --- | --- | --- |
| 데이터 저장 | 리프 노드, 브랜치 노드 모두 데이터 저장 가능 | 오직 리프 노드에만 데이터 저장 가능 |
| 트리의 높이 | 높음 | 낮음 (한 노드당 key를 많이 담을 수 있음) |
| 풀 스캔 시, 검색 속도 | 모든 노드 탐색 | 리프 노드에서 선형 탐색  |
| 키 중복 | 없음 | 있음(리프 노드에 모든 데이터가 있기 때문) |
| 검색 | 자주 access 되는 노드를 루트 노드 가까이 배치할 수 있고, 루트 노드에서 가까울 경우, 브랜치 노드에도 데이터가 존재하기 때문에 빠름 | 리프 노드까지 가야 데이터 존재 |
| 링크드 리스트 | 없음 | 리프 노드끼리 링크드 리스트로 연결 |

<br>
<br>

### 참고
- [https://yoongrammer.tistory.com/73](https://yoongrammer.tistory.com/73)
- [https://velog.io/@emplam27/자료구조-그림으로-알아보는-B-Tree](https://velog.io/@emplam27/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-B-Tree)
- [https://velog.io/@emplam27/자료구조-그림으로-알아보는-B-Plus-Tree](https://velog.io/@emplam27/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-B-Plus-Tree)
- [https://sdesigner.tistory.com/79](https://sdesigner.tistory.com/79)
- [https://zorba91.tistory.com/293](https://zorba91.tistory.com/293)