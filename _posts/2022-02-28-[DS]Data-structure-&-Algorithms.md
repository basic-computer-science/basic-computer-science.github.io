---
layout: post
title:  "[Data Structure] Data structure and Algorithms"
date:   2022-02-28 22:24:00 +0900
categories: DataStructure
---

### **๐ ๊ธฐ์ ๋ฉด์  ์ง๋ฌธ**

**Q.ย ์๋ฃ๊ตฌ์กฐ์ ์ ์์ ์ค์ํ ์ด์ ๋ฅผ ์ค๋ชํ์ธ์**

**Q.ย ์๋ฃ๊ตฌ์กฐ๋? /ย ์๊ณ ๋ฆฌ์ฆ์ด๋?**

**Q.ย ์๊ณ ย ์๋ ์๋ฃ๊ตฌ์กฐ์ ์ข๋ฅ์ย ย ๋ํด ์ด์ผ๊ธฐํด๋ณด์ธ์.**

**Q.ย ๋น์คย ํ๊ธฐ๋ฒ์ ๋ํด์ ์ค๋ชํด์ฃผ์ธ์.**

<br/>

---

<br/>

## **์๋ฃ๊ตฌ์กฐ๋ ๋ฌด์์ธ๊ฐ?**

> ๋ง ๊ทธ๋๋ ์๋ฃ๋ฅผ ์ ์ฅํ๋ ๊ตฌ์กฐ๋ก, ๋๋์ ๋ฐ์ดํฐ๋ฅผ ํจ์จ์ ์ผ๋ก ๊ด๋ฆฌํ  ์ ์๊ฒ๋ ํ๋ ๋ฐ์ดํฐ์ ๊ตฌ์กฐ.

-   ๋ฌธ์ ์ ํด๊ฒฐ์ ์ํด ์ฌ๋ฌ ๊ฐ์ง ํํ์ ์๋ฃ๊ตฌ์กฐ๊ฐ ํ์ฉ๋๋ค.
-   **ํํ์ ๋ฐ๋ผ ์ฅ๋จ์ ์ด ์กด์ฌ**ํ๋ฉฐ, ๊ตฌํํ๊ณ ์ ํ๋ ํ๋ก๊ทธ๋จ์ ์ฑ๋ฅ์ ๊ณ ๋ คํ์ฌ ์๋ง์ ์๋ฃ๊ตฌ์กฐ๋ฅผ ์ ํํด์ผ ํ๋ค.
-   ์๋ฃ๋ฅผ ํจ์จ์ ์ผ๋ก ๋ด๊ธฐ ์ํด ์๋ฃ๊ตฌ์กฐ๋ฅผ ๋ฐฐ์ฐ๋ฉฐ, ํ๋ก๊ทธ๋จ์์ ํน์  ์๊ณ ๋ฆฌ์ฆ์ ๊ตฌํํ๊ธฐ ์ํดย **์ ์ ํ ์๋ฃ๊ตฌ์กฐ๋ฅผ ์ฌ์ฉ**ํด์ผย **์ข์ ์ฑ๋ฅ**์ ๋ผ ์ ์๋ค.

<br/>

#### **์๋ฃ๊ตฌ์กฐ์ ๋ถ๋ฅ**

![์ฒจ๋ถ1_์๋ฃ๊ตฌ์กฐ์ ๋ถ๋ฅ](https://user-images.githubusercontent.com/100582309/157254136-499a77cb-48bd-4e51-9fd4-db8059054bf8.png)

-   ๋จ์ ๊ตฌ์กฐ : ํ๋ก๊ทธ๋๋ฐ์์ ์ฌ์ฉ๋๋ ๊ธฐ๋ณธ ๋ฐ์ดํฐ ํ์
-   ์ ํ ๊ตฌ์กฐ : ์ ์ฅ๋๋ ์๋ฃ์ ์ ํ๊ด๊ณ๊ฐ 1:1 (๋ฆฌ์คํธ, ์คํ, ํ)
-   ๋น์ ํ ๊ตฌ์กฐ :ย ๋ฐ์ดํฐ ํญ๋ชฉ ์ฌ์ด์ ๊ด๊ณ๊ฐย 1:nย ๋๋ย n:m (ํธ๋ฆฌ,ย ๊ทธ๋ํ ๋ฑ)
-   ํ์ผ ๊ตฌ์กฐย :ย ์๋ก ๊ด๋ จ๋ ํ๋๋ค๋ก ๊ตฌ์ฑ๋ ๋ ์ฝ๋์ ์งํฉ์ธ ํ์ผ์ ๋ํ ์๋ฃ๊ตฌ์กฐ

<br/><br/>

## **์๊ณ ๋ฆฌ์ฆ์ด๋ ๋ฌด์์ธ๊ฐ?**

์๋ฃ๊ตฌ์กฐ๊ฐ ์๋ฃ์ ํํ ๋ฐ ์ ์ฅ๋ฐฉ๋ฒ์ย ๋ปํ๋ค๋ฉด, ์๊ณ ๋ฆฌ์ฆ์ ์ด๋ ๊ฒ ํํ ๋ฐ ์ ์ฅ๋ ๋ฐ์ดํฐ๋ฅผ ํ์ฉํ์ฌ ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ๋ ๋ฐฉ๋ฒ์ ์๋ฏธํ๋ค.

#### **์๊ฐ ๋ณต์ก๋์ ๊ณต๊ฐ ๋ณต์ก๋**

-   **์๊ฐ ๋ณต์ก๋(Time complexity)**ย :ย  ์๊ณ ๋ฆฌ์ฆ์ ์คํ์์ผ ์๋ฃํ๋ ๋ฐ ํ์ํ ์ปดํจํฐ ์๊ฐ์ ์

> ์๊ฐ ๋ณต์ก๋ ๊ตฌํ๋ ๋ฒ : '๋ฐ์ดํฐ์ ์ n์ ๋ํ ์ฐ์ฐ ํ์์ ํจ์ T(n)์ ๊ตฌ์ฑํ๋คโ.ย   
> ย  ย  ย  ย  ย  ย  ย  ย  ย  ย  ย  ย  ย  ย  ย  ย ย โ ๋ฐ์ดํฐ ์์ ์ฆ๊ฐ์ ๋ฐ๋ฅธ ์ฐ์ฐ ํ์์ ๋ณํ ์ ๋๋ฅผ ํ๋จํ  ์ ์๋ค.

-   ๊ณต๊ฐ ๋ณต์ก๋(space complexity) : ์๊ณ ๋ฆฌ์ฆ์ ์คํ์์ผ ์๋ฃํ๋ ๋ฐ ํ์ํย **๋ฉ๋ชจ๋ฆฌ์ ์**

<br/>

#### **๋น-์ค ํ๊ธฐ๋ฒ(Big-Ohย Notation)**

์๊ณ ๋ฆฌ์ฆ์ ์๊ฐ ๋ณต์ก๋๋ฅผ ๋ํ๋ด๋ ์ํ์ ์ธ ํ๊ธฐ๋ฒ์ด๋ฉฐ, O(f(n))์ผ๋ก ๋ํ๋ธ๋ค.

![์ฒจ๋ถ2_๋น์คํ๊ธฐ๋ฒ](https://user-images.githubusercontent.com/100582309/157254681-a6b4ef54-b4ad-44d9-bfbc-7716fd1876c0.jpg)

-   O(1) : ์์ํ ๋น-์ค, ๋ฐ์ดํฐ ์์ ์๊ด์์ด ์ฐ์ฐํ์๊ฐ ๊ณ ์  (ex: push, pop)
-   O(log n) : ๋ก๊ทธํ ๋น-์ค, '๋ฐ์ดํฐ ์์ ์ฆ๊ฐ์จ'์ ๋นํด์ '์ฐ์ฐ ํ์์ ์ฆ๊ฐ์จ'์ด ํจ์ฌ ๋ฎ์ (ex: ์ด์งํธ๋ฆฌ)
-   O(n) : ์ ํ ๋น-์ค, ๋ฐ์ดํฐ์ ์์ ์ฐ์ฐํ์๊ฐ ๋น๋กํ๋ ์๊ณ ๋ฆฌ์ฆ์ด๋คย  (ex: for๋ฌธ)
-   O(n\*log n) : ๋ฐ์ดํฐ์ ์๊ฐ ๋ ๋ฐฐ๋ก ๋ ๋, ์ฐ์ฐ ํ์๋ ๋ ๋ฐฐ๋ฅผ ์กฐ๊ธ ๋๊ฒ ์ฆ๊ฐย  (ex: ํต ์ ๋ ฌ, ๋ณํฉ ์ ๋ ฌ, ํ ์ ๋ ฌ)
-   O(nยฒ) : ๋ฐ์ดํฐ ์์ ์ ๊ณฑ์ ํด๋นํ๋ ์ฐ์ฐ ํ์๋ฅผ ์๊ตฌ
-   O(nยณ) : ๋ฐ์ดํฐ ์์ ์ธ์ ๊ณฑ์ ํด๋นํ๋ ์ฐ์ฐ ํ์๋ฅผ ์๊ตฌ
-   O(2โฟ) : ์ง์ํ, ์ฌ์ฉํ๊ธฐ์ ๋ฌด๋ฆฌ๊ฐ ์์ผ๋ฉฐ ๋ฐ์ดํฐ ์ฆ๊ฐ์ ๋ฐ๋ผ ์ฐ์ฐ ํ์๊ฐ ๊ธฐํ๊ธ์๋ก ๋์ด๋จ (ex: ํผ๋ณด๋์น ์)

์ง๊ธ๊น์ง ์ค๋ชํ ๋น-์ค ํ๊ธฐ๋ค์ ์ฑ๋ฅ(์ํ ์๊ฐ, ์ฐ์ฐ ํ์)์ ๋์๋ฅผ ์ ๋ฆฌํ๋ฉด ๋ค์๊ณผ ๊ฐ๋ค

> O(1) < O(log n) < O(n) < O(n\*log n) < O(nยฒ) < O(nยณ) < O(2โฟ)

<br/>

## **์๋ฃ๊ตฌ์กฐ์ ์๊ณ ๋ฆฌ์ฆ**

์๋ฃ๊ตฌ์กฐ : ๋ฐ์ดํฐ๋ฅผ ์ด๋ ํ ํํ๋ก ์ ์ฅํ๊ณ  ๊ด๋ฆฌํ  ๊ฒ์ธ์ง์ ๋ํ ๋ฐฉ๋ฒ.  
**โ ๊ทธ๋์ ์๋ฃ๊ตฌ์กฐ๋ ์ด๋ป๊ฒ ํจ์จ์ ์ผ๋ก ์๋ฃ๋ฅผ ์ ์ฅํ  ๊ฒ์ธ๊ฐ์ ๋ํ ๊ณ ๋ฏผ์ด ํ์ํ๋ค.**

์๊ณ ๋ฆฌ์ฆ : ์ ์ฅ๋ ๋ฐ์ดํฐ๋ฅผ ์ฐพ๊ฑฐ๋ ๋ณํํ๊ฑฐ๋ ์์ ํ  ๋ ํ์ํ ๋ฐฉ๋ฒ.  
****โ**ย ๊ทธ๋์ ์๊ณ ๋ฆฌ์ฆ์ย ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํ ์ ์ฐจ์ ๋ํ ๊ณ ๋ฏผ์ด ํ์ํ๋ค.**

> **ํ๋ก๊ทธ๋จ =ย ์๋ฃ๊ตฌ์กฐย +ย ์๊ณ ๋ฆฌ์ฆ**

ํ๋ก๊ทธ๋จ์ ์ผ๋ฐ์  ์ ์๋ ๋ฐ์ดํฐ๋ฅผ ์ฒ๋ฆฌํ์ฌ ํน์  ์์์ ์ํํ๋ ์ผ๋ จ์ ๋ช๋ น์ด๋ค์ ๋ชจ์์ด๋ค. ์ฆ, ๋ฐ์ดํฐ๋ฅผ ์ฒ๋ฆฌํ  ๋์ ์๋ฃ๊ตฌ์กฐ๊ฐ ์ฌ์ฉ๋๊ณ , ์ฃผ์ด์ง ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด ํน์  ์์์ ์ํํ๋ ์ ์ฐจ๋ ์๊ณ ๋ฆฌ์ฆ์ธ ๊ฒ์ด๋ค. ๋ฐ๋ผ์, ํ๋ก๊ทธ๋จ์ ์๋ฃ๊ตฌ์กฐ์ ์๊ณ ๋ฆฌ์ฆ์ผ๋ก ์ด๋ฃจ์ด์ง๋ค๊ณ  ํํ๋๊ณ ๋ ํ๋ค.

ex : ์ต๋๊ฐ์ ์ฐพ๋ ํ๋ก๊ทธ๋จ = ๋ฐฐ์ด(์๋ฃ๊ตฌ์กฐ) + ์์ฐจ ํ์(์๊ณ ๋ฆฌ์ฆ)

<br/>

---

<br/>

### **๐ ๊ธฐ์ ๋ฉด์  ์ง๋ฌธ (๋ตํด๋ณด๊ธฐ)**

**Q.ย ์๋ฃ๊ตฌ์กฐ์ ์ ์์ ์ค์ํ ์ด์ ๋ฅผ ์ค๋ชํ์ธ์**

-   ๋ง ๊ทธ๋๋ ์๋ฃ๋ฅผ ์ ์ฅํ๋ ๊ตฌ์กฐ
-   ๋๋์ ๋ฐ์ดํฐ๋ฅผ ํจ์จ์ ์ผ๋ก ๊ด๋ฆฌํ  ์ ์๊ฒ๋ ํ๋ ๊ตฌ์กฐ. ๋ฌธ์ ์ ํด๊ฒฐ์ ์ํด ์ฌ๋ฌ ๊ฐ์ง ํํ์ ์๋ฃ๊ตฌ์กฐ๊ฐ ํ์ฉ๋๋ค.
-   ํํ์ ๋ฐ๋ผ ์ฅ๋จ์ ์ด ์กด์ฌํ๋ค.
-   ํ๋ก๊ทธ๋จ์์ ํน์  ์๊ณ ๋ฆฌ์ฆ์ ๊ตฌํํ๊ธฐ ์ํดย ์ ์ ํ ์๋ฃ๊ตฌ์กฐ๋ฅผ ์ฌ์ฉํด์ผย ์ข์ ์ฑ๋ฅ์ ๋ผ ์ ์๋ค.

**Q.ย ์๋ฃ๊ตฌ์กฐ๋? /ย ์๊ณ ๋ฆฌ์ฆ์ด๋?**

-   **์๋ฃ๊ตฌ์กฐ** : ๋ฐ์ดํฐ๋ฅผ ์ด๋ ํ ํํ๋ก ์ ์ฅํ๊ณ  ๊ด๋ฆฌํ  ๊ฒ์ธ์ง์ ๋ํ ๋ฐฉ๋ฒ.
    -   ๊ทธ๋์ ์๋ฃ๊ตฌ์กฐ๋ย ์ด๋ป๊ฒ ํจ์จ์ ์ผ๋ก ์๋ฃ๋ฅผ ์ ์ฅํ  ๊ฒ์ธ๊ฐ์ ๋ํ ๊ณ ๋ฏผ์ด ํ์ํ๋ค
-   **์๊ณ ๋ฆฌ์ฆ**ย : ์ ์ฅ๋ ๋ฐ์ดํฐ๋ฅผ ์ฐพ๊ฑฐ๋ ๋ณํํ๊ฑฐ๋ ์์ ํ  ๋ ํ์ํ ๋ฐฉ๋ฒ.
    -   ๊ทธ๋์ ์๊ณ ๋ฆฌ์ฆ์ ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํ ์ ์ฐจ์ ๋ํ ๊ณ ๋ฏผ์ด ํ์ํ๋ค.

**Q**.ย ์๊ณ ย ์๋ ์๋ฃ๊ตฌ์กฐ์ ์ข๋ฅ์ย ย ๋ํด ์ด์ผ๊ธฐํด๋ณด์ธ์.

-   List(๋ฆฌ์คํธ), Linked List(๋งํฌ๋ ๋ฆฌ์คํธ), Array(๋ฐฐ์ด), Stack(์คํ), Queue(ํ), Dequeue(๋ํ), Tree(ํธ๋ฆฌ), Heap(ํ), Graph(๊ทธ๋ํ)

**Q.ย ๋น์คย ํ๊ธฐ๋ฒ์ ๋ํด์ ์ค๋ชํด์ฃผ์ธ์.**

-   ์๊ณ ๋ฆฌ์ฆ์ ํจ์จ์ฑ์ ํ๊ธฐํด์ฃผ๋ ํ๊ธฐ๋ฒ. ๋ณดํต ์๊ณ ๋ฆฌ์ฆ์ ์๊ฐ ๋ณต์ก๋์ ์ฃผ๋ก ์ฌ์ฉ๋๋ค.