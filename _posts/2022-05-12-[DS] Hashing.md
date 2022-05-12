---
layout: post
title:  "[Data Structure] Hashing"
date:   2022-05-12 10:00:00 +0900
categories: DataStructure
---

📌 **관련 용어 정리**
    <br>
    **Hash** : 데이터를 다루는 기법 중 하나
    <br>
    **Hash Table** :  키 값의 연산에 의해 직접 접근이 가능한 **구조**
    <br>
    **Hash Function** : 데이터를 효율적으로 관리하기 위해서 임의의 길이의 데이터를 고정된 길이의 데이터로 매핑하는 **함수**
    <br>
    **Hashing** : 키(key)와 값(value)으로 **매핑되는 과정** 자체
    {: .notice}

<br>

# Hashing

> 해싱
> 

---

임의의 데이터를 해시함수를 사용하여 고정된 크기의 값으로 변환하는 작업을 말한다.

💡 **해싱이 나오게 된 배경**
    <br>
대부분의 탐색 방법들은 탐색 키를 저장된 키값과 반복적으로 비교하면서 탐색을 원하는 항목에 접근한다. 반면 **해싱은 키 값에 직접 산술적인 연산을 적용**하여 항목이 저장되어 있는 **테이블의 주소를 계산하여 항목에 접근**한다. 
        <br>
***다른 탐색*** : 키들의 비교에 의한 탐색은 정렬이 안 되어 있다면 `O(n)`이고 정렬이 되어 있다면 `O(logn)`이다.
        <br>
***해싱*** : 더욱 빠른 탐색을 필요로 할 때에 사용된다. 해싱은 이론적으로 `O(1)`의 시간 안에 탐색을 끝마칠 수도 있다. 해싱은 보통 사전(dictionary)과 같은 자료 구조를 구현할 때에 최상의 선택이 된다.
{: .notice}


<br>

# **Hash Function**

> 해시 함수
>

---

임의의 길이의 데이터를 고정된 길이의 데이터로 매핑하는 함수이다. 해시 함수에 의해 얻어지는 값은 해시 값, 해시 코드, 해시 체크섬 또는 간단하게 해시라고 한다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/69dad6c0-114d-4aa2-a268-d49fdaf87935/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220512%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220512T121946Z&X-Amz-Expires=86400&X-Amz-Signature=a8ea696d7daf0822bc1fba3a150c19acbffdc67bb57d13bc140cd8afb7856514&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

<br>

# **Hash Table**

> 해시 테이블
> 

---

해싱을 사용하여 (Key, Value)로 데이터를 저장하는 자료구조 중 하나로 빠르게 데이터를 검색할 수 있다는 특징이 있다. 해시 테이블이 빠른 검색속도를 제공하는 이유는, 내부적으로 배열(bucket)을 사용하여 데이터를 저장하기 때문이다. 해시 테이블은 각각의 key값에 해시 함수를 적용하여 배열의 고유한 index를 생성하고, 이 index를 활용해 값을 저장하거나 검색하게 된다. 실제 값이 저장되는 장소를 bucket(또는 Slot) 이라고 한다.

![https://blog.kakaocdn.net/dn/ceKgGz/btqAUvLYrPN/DMVl0lwN8tA2hobFxqHcf0/img.png](https://blog.kakaocdn.net/dn/ceKgGz/btqAUvLYrPN/DMVl0lwN8tA2hobFxqHcf0/img.png)

👉🏽 **원래 데이터의 값(Key)   ➡   Hash Function   ➡   Hash Function의 결과 (=Hash Code)   ➡   Hash Code를 배열의 Index 로 사용   ➡   해당하는 Index에 data 넣기**
{: .notice}

예를 들어 우리가 (Key, Value)가 ("John Smith", "521-1234")인 데이터를 크기가 16인 해시 테이블에 저장한다고 하자. 그러면 먼저 index = hash_function("John Smith") % 16 연산을 통해 index 값을 계산한다. 그리고 array[index] = "521-1234" 로 전화번호를 저장하게 된다.

이런식으로 데이터를 저장하다 보면 계산된 인덱스 값이 중복될 수가 있다. 예를 들어 저장하고자 하는 키가 정수이고 hash_function이 key%10이다. 전체 사이즈가 10일때, key 1,11,21은 같은 인덱스 값을 가지게 된다. **충돌 (collision)**이라고한다.

### 📌 **장점**
- 적은 리소스로 많은 데이터를 효율적으로 관리할 수 있다.
  <br>

  👉🏽  HDD, Cloud에 존재하는 많은 데이터들을 유한한 개의 해시(Hash)값으로 매핑하여 작은 크기의 캐쉬 메모리로 프로세스 관리가 가능하다.{: .notice}
- key-value 가 1:1 로 매핑되어 있기 때문에 삽입, 삭제, 검색의 과정이 모두 평균적으로 `O(1)`의 시간복잡도를 가지고 있다.
- 중복을 제거하는데 유용하다.
- 키(key)와 해시값(Hash)이 연관성이 없어 보안에도 많이 사용된다.
- 데이터 캐싱(Data Caching)에 많이 사용된다.
    <br>

    👉🏽 get(key), put(key)에 캐시 로직을 추가하면 자주 hit하는 데이터를 바로 찾을 수 있다.
    {: .notice}
    

### 📌 **단점**

- 해시 충돌이 발생한다.
- 순서/관계가 있는 배열에는 어울리지 않는다.
- 공간 효율성이 떨어진다. -> 데이터가 저장되기 전에 미리 저장공간을 확보해야 하기 때문이다.
- hash function의 의존도가 높다 => 해시함수가 복잡한 경우 연산속도 증가

<br>

# Hash Function

> 해시 함수
> 

---

해시 함수에서 중요한 것은 고유한 인덱스 값을 설정하는 것이다. 해시 테이블에 사용되는 대표적인 해시 함수로는 아래의 3가지가 있다.

1. **Division Method** : 나눗셈을 이용하는 방법으로 입력값을 테이블의 크기로 나누어 계산한다. (주소 = 입력값 % 테이블의 크기) 테이블의 크기를 소수로 정하고 2의 제곱수와 먼 값을 사용해야 효과가 좋다고 알려져 있다.
2. **Digit Folding** : 각 Key의 문자열을 ASCII 코드로 바꾸고 값을 합한 데이터를 테이블 내의 주소로 사용하는 방법이다.
3. **Multiplication Method** : 곱하기를 이용하는 방법으로, 숫자로 된 Key값 x와 0과 1사이의 실수 A, 보통 2의 제곱수인 m을 사용하여 다음과 같은 계산을 해준다.
    
    ![Untitled](https://user-images.githubusercontent.com/100582309/168075085-ccae0ea7-d7c5-4aad-9174-f86786f5d97f.png)
    
    ![Untitled](https://user-images.githubusercontent.com/100582309/168075160-7a9873aa-57d2-4119-a741-f23c84ef4018.png)
    
4. **Universal Hashing** : 다수의 해시함수를 만들어 집합 H에 넣어두고, 무작위로 해시함수를 선택해 해시값을 만드는 기법이다.

<br>

# **Collision**

> 충돌
> 

---

서로 다른 문자열이 Hash function을 통해 Hash 한 결과가 같은 경우 (중복되는 경우)이다.충돌을 줄여주는 좋은 해시 함수를 사용하는 것이 좋다. 왜냐하면 충돌이 많아질 수록 탐색의 시간 복잡도가 O(1)에서 O(n)에 가까워지기 때문이다. 해결 방법은 다음과 같다.

1. **Separating Chaining** - Linked List, Tree(Red-Black Tree)
2. **Open addressing** - Linear Probing, Quadratic Probing, Double hashing

<br>

## 1. **Separating Chaining**

동일한 버킷의 데이터에 대해 자료구조를 활용해 추가 메모리를 사용하여 다음 데이터의 주소를 저장하는 것이다. 위의 그림과 같이 동일한 버킷으로 접근을 한다면 데이터들을 연결을 해서 관리해주고 있다. 실제로 Java8의 Hash테이블은 Self-Balancing Binary Search Tree 자료구조를 사용해 Chaining 방식을 구현하였다.

Separating Chaining은 JDK(Java Development Kit) 내부에서도 사용하고 있는 충돌 처리 방식인데, 이를 기준으로 설명해보겠다. 이때 활용되는 자료구조는 Linked List와 Tree(Red-Black Tree)이다.

두 가지 자료구조를 활용하는 방법은, data가 6개 이하이면 linked list를 사용하고 8개 이상이면 tree를 사용한다. (왜 7개가 아니냐 의문이 들 수 있다. 만약 7개일 때, 데이터를 삭제하게 되면 linked list로 바꿔야 하고 만약 추가되면 tree로 바꿔야한다. 이때 바꾸는데 오버헤드가 있기 때문에 기준이 6, 8이다.)

![Separating Chaining - Linked list](https://blog.kakaocdn.net/dn/dzUUKL/btqAU5TNqPb/57l6XNBJBLFlL1xcXCePq1/img.png)

Separating Chaining - Linked list

위 그림은 Linked list를 사용한 것이다. 인덱스가 충돌이 날 때, 인덱스가 가리키고 있는 linked list에 노드를 추가하여 값을 삽입한다. 데이터를 탐색할 때는 키에 대한 인덱스가 가리키고 있는 Linked list를 선형 검색하여, 해당 키에 대한 데이터를 반환한다. 삭제하는 것도 비슷하게, 키에 대한 인덱스가 가리키고 있는 Linked list에서, 그 노드를 삭제한다. Separate changing 방식은 Linked List 구조를 사용하고 있기 때문에, 추가할 수 있는 데이터 수의 제약이 작다.

이러한 Chaining 방식은 해시 테이블의 확장이 필요없고 간단하게 구현이 가능하며, 손쉽게 삭제할 수 있다는 장점이 있다. 하지만 데이터의 수가 많아지면 동일한 버킷에 chaining되는 데이터가 많아지며 그에 따라 캐시의 효율성이 감소한다는 단점이 있다.

<br>

## 2. **Open addressing**

추가적인 메모리를 사용하는 Chaining 방식과 다르게 비어있는 해시 테이블의 공간을 활용하는 방법이다. 추가적인 메모리 공간을 사용하지 않기 때문에 Separate chaining방식에 비해서 메모리를 덜 사용한다. Open Addressing을 구현하기 위한 대표적인 방법으로는 3가지 방식이 존재한다.

1. **Linear Probing**
2. **Quadratic Probing**
3. **Double Hashing Probing**

<br>

### 1) Linear Probing

현재의 버킷 index로부터 고정폭만큼씩 이동하여 차례대로 검색해 비어 있는 버킷에 데이터를 저장한다.

![Open addressing - Linear Probing](https://blog.kakaocdn.net/dn/ALk2h/btqAWxowTOl/oWIIt6DZ7jdYBd3jngUqWk/img.png)

Open addressing - Linear Probing

Linear Probing방식은 인덱스가 중복되는 충돌이 발생할 때 인덱스 뒤에 있는 버킷중에 빈 버킷을 찾아서 데이터를 넣는다. 그림에서 Sandra(key)의 인덱스는 152이다. 하지만 키 John과 충돌이 나기 때문에 그 다음 인덱스인 153에 데이터를 넣는다.

Linear Probing 방식에서의 탐색은 Sandra(key)에 대해서 검색을 하면, index가 152이기 때문에, key가 일치하지 않기 때문에 뒤의 index를 검색해서 같은 키가 나오거나 또는 Key가 없을때 까지 검색을 진행한다. 삭제는 더미 노드를 넣어서 검색할 때 다음 인덱스까지 검색을 연결해주는 역할을 해줘야한다. (즉, 삭제가 어렵다.)

💡 **Dummy Node**
<br>
연결 리스트를 구현 하는 방법은 두 가지가 있는데, 하나는 더미 노드(Dummy Node)를 이용하는 방법이고 하나는 헤드 포인터(Head Pointer)를 사용하는 방법이다.
<br>
더미 노드란, 데이터 저장을 위한 노드가 아니라 노드의 추가/삭제 구현의 간편성을 위해 사용하는 노드이다. 실제 데이터를 저장하는 다른 노드를 가리키는 포인터의 역할만을 수행한다. 반면 헤드 포인터란, 연결 리스트의 첫 번째 노드를 가리키는 포인터이다. 더미 노드를 사용하는 방법이 좀 더 구현이 편하고 고려해야 할 수가 적다.
{: .notice}

<br>

### 2) **Quadratic Probing**

해시의 저장순서 폭을 제곱으로 저장하는 방식이다. 예를 들어 처음 충돌이 발생한 경우에는 1만큼 이동하고 그 다음 계속 충돌이 발생하면 $2^2$, $3^2$ 칸씩 옮기는 방식이다.

![ ~ ](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1f3bbe8e-d241-4094-a784-3e19446cefbc/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220512%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220512T124228Z&X-Amz-Expires=86400&X-Amz-Signature=fe44a975cf99003f349a95ef56ee00577f1a87d60b199f033a13e149fc3920db&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

<br>

### 3) **Double Hashing Probing**

해시된 값을 한번 더 해싱하여 해시의 규칙성을 없애버리는 방식이다. 해시된 값을 한번 더 해싱하여 새로운 주소를 할당하기 때문에 다른 방법들보다 많은 연산을 하게 된다.

<br>

## 💡 Resizing

Separate chaning에 경우, 버킷이 일정 수준으로 차 버리면 각 버킷에 연결되어 있는 List의 길이가 늘어나기 때문에, 검색 성능이 떨어지기 때문에 버킷의 개수를 늘려줘야 한다. 또한 Open addressing의 경우, 고정 크기 배열을 사용하기 때문에 데이터를 더 넣기 위해서는 배열을 확장해야 한다. 이를 **리사이징(Resizing)이라고** 한다.

보통 두 배로 확장하는데, 확장하는 임계점은 현재 데이터 개수가 해시 버킷의 개수의 75%가 될 때이다. 0.75라는 숫자는 **load factor**라고 불린다. 리사이징은 더 큰 버킷을 가지는 array를 새로 만든 다음에, 다시 새로운 array에 hash를 다시 계산해서 복사해줘야 한다.

<br><br>

### 참고자료

- [https://hee96-story.tistory.com/48](https://hee96-story.tistory.com/48)
- [https://mattlee.tistory.com/62](https://mattlee.tistory.com/62)
- [https://mhlee.tistory.com/3](https://mhlee.tistory.com/3)
- [https://mangkyu.tistory.com/102](https://mangkyu.tistory.com/102)
- [https://itstory.tk/entry/해슁Hashing-해쉬-알고리즘-해쉬-함수](https://itstory.tk/entry/%ED%95%B4%EC%8A%81Hashing-%ED%95%B4%EC%89%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%ED%95%B4%EC%89%AC-%ED%95%A8%EC%88%98)