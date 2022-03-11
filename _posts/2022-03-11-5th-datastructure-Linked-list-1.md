---
layout: post
title:  "Linked List (1)"
date:   2022-03-11 19:00:00 +0900
categories: DataStructure
---

<br/>

### **📌기술면접 질문**

**Q. Linked List에 대해 설명해보세요.**

더보기

Linked List는 데이터와 다음 데이터를 가리키는 정보를 저장하고 있는 자료구조 입니다. 데이터를 추가 하기에 좋지만 데이터를 찾을(탐색) 때 순차적으로 찾아야 하기 때문에 검색 하는 것은 느립니다.

그래서 데이터의 입출력이 많을 때 Linked List를 사용 하는 것이 좋습니다.

**Q. Array와 Linked List의 차이가 무엇인가요? (N사 전화면접)**

더보기

**Array List**는 데이터들이 순서대로 늘어선 배열의 형식을 취하고 있지만, **Linked List**는 자료의 주소값으로 서로 연결된 형식을 가지고 있다.  
  
**\- Array List**  
    - 원하는 데이터에 무작위로 접근할 수 있다. (Random Access 가능)  
    - 리스트의 크기가 제한되어 있으며, 리스트의 크기를 재조정하는 것은 많은 연산이 필요하다.  
         -> (새로운 메모리 블럭을 찾아야 하기 때문)  
    - 데이터의 추가/삭제를 위해서는 임시 배열을 생성하여 복제하고 있어 시간이 오래 걸린다.  
  
**\- Linked List**  
    - 리스트의 크기에 영향 없이 데이터를 추가할 수 있다. -> (연속적 메모리 할당이 아니기 때문)  
    - 데이터를 추가하기 위해 새로운 노드를 생성하여 연결하므로 추가/삭제 연산이 빠르다.  
    - 무작위 접근이 불가능하며, 순차 접근만이 가능하다. ->(일반적으로 이전 노드에 포인터가 있다.)

더보기

|   | **Array** | **Linked List** |
| --- | --- | --- |
| 데이터 접근 방식 | Random Access   (인덱스 통해 직접 접근 가능) | Sequential Access   (순차적 검색하여 탐색) |
| 특정 데이터에 접근하는 시간복잡도 | O(1) | O(N) |
| 데이터 저장방식 | 연속적 메모리 할당   (데이터가 인접 메모리 위치에 연이어 저장) | 새로운 요소에 할당된 메모리 위치 주소가 이전요소에 저장 |
| 삽입, 삭제 시간복잡도 | O(N) | O(1) |
| 메모리 할당 | 정적 메모리 할당   (선언시 컴파일 타임에 할당)   (즉, 선언시 크기가 고정) | 동적 메모리 할당   (데이터 추가시 런타임에 메모리 할당)   (즉, 데이터 삽입시마다 증가) |
| Stack 섹션에 메모리 할당 | Heap 섹션에 메모리 할당 |

<br/><br/>

# **Linked List란?**

---

> 💡 연결리스트와 배열은 서로의 장단점을 보완해주는 관계이기 때문에, 보통 연결리스트를 설명할 때에는 배열과 묶어서 설명한다.

<br/>

## **Array**

> 연속적인 공간에 순차적으로 데이터를 저장하는 구조

![다운로드](https://user-images.githubusercontent.com/100582309/157840534-d93aa6c0-d7f6-4b86-9b37-b721430672e6.png)

**장점**

-   배열은 데이터들이 메모리 공간에서 연속적으로 저장되어 있는 자료구조이다. 이 특징 덕분에 **index를 통한 접근이 빠르고 간편하다**는 특징이 있다.

**단점** 

-   배열을 사용하기 위해서는 **처음에 배열의 크기를 선언해야 하고, 크기의 수정이 불가능**하기 때문에 **메모리 사용이 비효율적**이다. 또, **무한에 가까운 자료를 저장하는 데에는 부적합**하다.
-   처음이나 마지막 부분이 아닌 중간 데이터의 삽입/삭제 과정에서는 **데이터들을 재배열하는 추가 작업**이 필요하다. 따라서 **데이터의 이동 및 복사가 매우 빈번**하게 일어난다는 큰 한계가 있다.

<br/>

## **Linked List**

> 자기 참조 구조체의 동적 메모리 연산을 이용한 자료구조

![다운로드1](https://user-images.githubusercontent.com/100582309/157840759-a4d87741-a659-46d2-9184-21c2a21f7470.jpg)

✔ 연결리스트는 순차적으로 데이터를 보관하는 것이 아닌, 떨어진 공간에 존재하는 데이터를 연결시켜둔 것이다.

✔ 포인터로 연결된 것이 특징이며, 선형 리스트에 비해 상대적으로 구현이 어렵다.

✔ 큐, 스택, 해시 테이블 등 다른 자료구조(추상자료형, ADT)의 기반이 된다.

**장점**

-   배열과 다르게, **필요한 경우 메모리에 공간을 할당해서 사용**한다.
-   **중간 데이터의 삽입/삭제 시에도 연결만 바꿔주면 되므로** 모든 값을 재배열하는 등의 추가작업이 필요하지 않다.

**단점** 

-   **순차적 탐색**으로 특정 위치의 데이터에 접근하기 때문에 배열에 비해 **탐색속도가 느리다**.
-   **현재 데이터의 다음 데이터에 대한 연결 정보까지 저장**해두어야 하기 때문에, 별도의 공간이 필요해서 **효율이 좋지 않다.**

<br/>

### **형태**

![다운로드 (1)](https://user-images.githubusercontent.com/100582309/157840837-c6fc9d4d-6b31-4f28-8a49-5ea6ced5c3fe.png)

**✔  {Node : (Data, link)}**가 하나의 단위로써 저장된다.

**✔**  리스트의 처음 : **Head node**  
    리스트의 끝    : **Tail node** = End node  
    \* 통상 묵시적으로, 데이터 없이 시작, 끝 만을 알림

<br/>

### **시간복잡도**

|   | Array | Linked List |
| --- | --- | --- |
| 탐색 (특정 데이터) | O(1) | O(1) |
| 삽입/삭제 | O(N) | O(1) |

<br/><br/>


## **Linked List의 연산**

---

### **삽입**

-   노드를 생성하고, 삽입하려는 위치를 찾는다.

![다운로드 (1)](https://user-images.githubusercontent.com/100582309/157840991-83fd3d46-8286-40d8-88df-c06f17d74217.jpg)

-   삽입할 노드를 NewNode라고 하면, NewNode가 그 다음 노드를 가리키게 한다.

```
NewNode.next -> RightNode
```

![5](https://user-images.githubusercontent.com/100582309/157841063-f9ad4284-183c-4482-9948-b08b78f20e2e.jpg)

-   새로 삽입한 노드의 이전 노드가 새로운 노드를 가리키게 한다.  

```
LeftNode.next -> NewNode
```

![다운로드 (1)](https://user-images.githubusercontent.com/100582309/157841146-55698342-36a9-4f27-ad79-7b3e44023225.jpg)

<br/>

### **삭제**

-   탐색을 통해 삭제하고자 하는 노드 (TargetNode)를 찾는다. 

![다운로드 (g1)](https://user-images.githubusercontent.com/100582309/157841870-b99336bd-dade-481c-a2ad-9d4b91bfe1fa.jpg)

-   TargetNode의 왼쪽 노드가 TargetNode의 오른쪽 노드를 가리키게 한다.

```
LeftNode.next -> TargetNode.next
```

![다운로드 (1g)](https://user-images.githubusercontent.com/100582309/157841934-1c3d5e6f-9b8c-4180-986d-4c42f23284d9.jpg)
-   TargetNode가 RightNode를 가리키지 않게 한다 TargetNode.next -> NULL

![다운로드 (1)](https://user-images.githubusercontent.com/100582309/157841978-76c9bc3b-3c76-4c73-aa47-dffefdd27f97.jpg)

<br/><br/>

# **Linked List의 종류**

## **Singly Linked List**

> 단순 연결 리스트

![vxdvd](https://user-images.githubusercontent.com/100582309/157842080-441acd94-3458-46b3-9bf3-01507ef79f5a.png)

-   위에서 설명했듯, 하나의 노드 안에는 위의 사진과 같이 '데이터'와 '포인터'로 구성되어있다.
-   다음 노드를 참조하는 주소 중 하나가 잘못되는 경우, 체인이 끊어진 것처럼 뒤쪽의 자료들과 연결이 끊기게 된다. 그러므로 안정적인 자료구조라고는 할 수 없을 것이다.
-   마지막 Node의 링크는, Null값을 가리킴

이를 보완하는 방법은 바로 다음에 설명할 이중 (양방향) 연결 리스트이다.

<br/>

## **Doubly Linked List**

> 이중 연결 리스트

![xvdv](https://user-images.githubusercontent.com/100582309/157842121-4958fd85-a2dd-4707-b6b8-60df140d10f9.png)

-    순차적 탐색이 필수적인 단방향 연결 리스트의 단점을 보완한 연결 리스트이다.
-   '다음 데이터의 주소 값' 그리고 '이전 데이터의 주소 값'을 가지고 있는 연결 리스트이다. 즉, 노드 안에 '**데이터 값'과 '이전, 이후 데이터의 주소 값'**을 가지고 있는 것이다.
-   그러므로 단순 연결 리스트와 다르게 자신이 원하는 데이터가 앞쪽에 가까우면 'head'부터, 뒤쪽에 가까우면 'tail'부터 찾아보면 더욱 빨리 찾을 수 있다.

<br/>

## **Circular Linked List**

> 원형 연결리스트

![dvxvd](https://user-images.githubusercontent.com/100582309/157842161-ee0130aa-275b-4160-8d7c-3d194f7ac7df.png)


-   마지막 Node의 링크가 리스트의 처음 Node를 가리킴
-   단순 연결 리스트의 처음과 끝을 연결하면 원형 연결 리스트가 됨

<br/><br/>


참고 :

더보기

-   개발자 Miro님의 블로그  : [https://jud00.tistory.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%97%B0%EA%B2%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8Linked-List](https://jud00.tistory.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%97%B0%EA%B2%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8Linked-List)

-   정보통신기술용어해설 : [http://www.ktword.co.kr/test/view/view.php?m\_temp1=3559](http://www.ktword.co.kr/test/view/view.php?m_temp1=3559)