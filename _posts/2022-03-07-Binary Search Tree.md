---
layout: post
title:  "Binary Search Tree"
categories: Algorithm
---

# Binary Search Tree

- 이진 탐색 트리 : 이진탐색 + 연결리스트 결합한 자료구조
    - 노드의 왼쪽 하위 트리에는 노드의 키보다 작은 키가있는 노드 만 포함
    - 노드의 오른쪽 하위 트리에는 노드의 키보다 큰 키가있는 노드 만 포함
    - 왼쪽 및 오른쪽 하위 트리도 각각 이진 검색 트리 여야한다.
    - If y is in left sub-tree of x, then key[y] ≤ key[x].
    If y is in right sub-tree of x, then key[y] ≥ key[x]

![Untitled](\public\img\Algorithm\Binary Sea bbb81\Untitled.png)

## Inorder tree walk

- Inorder tree walk(중위 순회)를 수행하여 모든 키를 정렬된 순서로 가져올 수 있다.
- left of x → x → right of x

🔽

INORDER-TREE-WALK(x) : 

if x ≠ NIL

then INORDER-TREE-WALK(left[x])

print key[x]

INORDER-TREE-WALK(right[x])

- Θ(n) time for a tree with n nodes ⇒ 각 노드들을 한번씩 방문하고 print하기 때문에

## Searching

🔽

TREE-SEARCH(x, k) : 

if x = NIL or k = key[x]

then return x

if k < key[x]

then return TREE-SEARCH(left[x], k)

else return TREE-SEARCH(right[x], k)

Initial call is TREE-SEARCH(root[T], k)

- 루트에서 시작
    
    → 검색 값을 루트와 비교. 루트보다 작으면 왼쪽에 대해 재귀하고 크다면 오른쪽으로 재귀 
    
    → 일치하는 값을 찾을 때까지 절차를 반복
    

## Minimum and Maximum

- BST의 minimum key는 leftmost node, maximum key는 rightmost node
- TREE-MINIMUN(x){
    
     while left[x] != NIL 
    
    do x <- left[x] 
    
    return x 
    
    }
    
- Tree-MAXIMUM(x){
    
    while right[x] != NIL 
    
    do x <- right[x] 
    
    return x 
    
    }
    

## Successor and Predecessor

- 노드 x의 Successor란 key[x]보다 크면서 가장 작은 키를 가진 노드 (**나보다 크면서 가장 작은 노드**!)
    - Predecessor 는 **나보다 작으면서 가장 큰 노드**! (Successor과 반대 의미)
- x가 BST의 Largest key 라면, x의 successor는 NIL
- two cases
    - node x의  right subtree : non-empty ⇒ x의 successor는 x의 right subtree의 minimum
    - node x의 right subtree: empty ⇒  어떤 노드 y의 왼쪽 부트리의 최대값이 x가 되는 그런 노드 y가 x의 Successor
- TREE-SUCCESSOR(x){
    
    if right[x] != NIL 
    
    then return TREE-MINIMUM(right[x]) 
    
    y <- p[x] 
    
    while y != NIL and x = right[y] 
    
    do x <- y 
    
    y <- p[x] 
    
    return y
    
    }
    

## Insertion

→ leaf node에 추가 (NIL)

🔽

TREE-INSERT(T, z){  // T : Tree, z : insert할 노드 

y <- NIL 

x <- root[T] 

while x != NIL 

do y <- x 

if key[z] < key[x] 

then x <- left[x] 

else x <- right[x] 

p[z] <- y 

if y = NIL 

then root[T] <- z 

else if key[z] < key[y] 

then left[y] <- z 

else right[y] <- z 

}

## Deletion

🔽

TREE-DELETE(T, z){ // T : tree, z : 삭제할 노드 

if left[z] = NIL or right[z] = NIL 

then y <- z 

else y <- TREE-SUCCESSOR(z) 

if left[y] != NIL 

then x <- left[y] 

else x <- right[y] 

if x != NIL 

then p[x] <- p[y] 

if p[y] = NIL 

then root[T] <- x 

else if y = left[p[y]] 

then left[p[y]] <- x 

else right[p[y]] <- x 

if y != z 

then key[z] <- key[y] 

copy y's satellite data into z 

return y 

}

- Case 1 : z가 자식이 없는 경우. 그냥 삭제하기
- Case 2 : z가 자식이 하나인 경우. 해당 노드의 부모-자식 노드를 연결
- Case 3 : z가 자식이 둘인 경우. → 중위순회방식으로 정렬한 후 successor값(삭제할 값 바로 다음 값)을 삭제한 위치에 채워넣는다. (predecessor값으로 넣어도 상관없음)
    
    ![Untitled](\public\img\Algorithm\Binary Sea bbb81\Untitled 1.png)
    

## 정리

- 탐색, 삽입, 삭제의 계산복잡성은 모두 O(h)=O(logn) → 트리의 높이에 의해 수행시간이 결정되는 구조 h=logn
- 아래 그림의 경우 노드 수는 적은 편인데 높이가 4. 균형이 안 맞기 때문이다. 극단적으로는 n개의 노드가 크기 순으로 일렬로 늘어뜨려져 높이 또한 n이 되는 경우도 이진트리탐색에 해당된다. 결과적으로 이진탐색트리의 계산복잡성은 O(n). 이래가지고서는 탐색 속도가 O(logn)으로 빠른 이진탐색을 계승했다고 보기 어렵다. 이 때문에 트리의 입력, 삭제 단계에 트리 전체의 균형을 맞추는 이진탐색트리의 일종인 AVL Tree가 제안되었다.
    
    ![Untitled](\public\img\Algorithm\Binary Sea bbb81\Untitled 2.png)
    

출처 

[https://ratsgo.github.io/data structure&algorithm/2017/10/22/bst/](https://ratsgo.github.io/data%20structure&algorithm/2017/10/22/bst/)

[https://yoongrammer.tistory.com/71](https://yoongrammer.tistory.com/71)

[https://journee912.tistory.com/59](https://journee912.tistory.com/59)