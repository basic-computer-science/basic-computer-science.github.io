---
layout: post
title:  "[Data Structure] Binary Search Tree"
date:   2022-04-18 18:00:00 +0900
categories: DataStructure
---

# **Binary Search Tree**

---

๐ก ์ด์งย <u>ํ์</u>ย ํธ๋ฆฌ๋ ์ด๋ฆ์์ ์ ์ ์๋ฏ์ด, ์ฝ์์ด๋ ์ญ์ ๋ณด๋ค๋ ํ์์ ์ฃผ ๋ชฉ์ ์ ๋ ์๋ฃ๊ตฌ์กฐ์ด๋ค. {: .notice}

<br>

## **Search Tree**

> ํ์ ํธ๋ฆฌ
> 
- skip lists์ hash table๋ค๊ณผ ๊ฐ๊ฑฐ๋ ๊ทธ ์ด์์ ์ฑ๋ฅ์ ๊ฐ์ง
- ์ฌ์ ์ ๊ตฌํํ๋ ๋ฐ์ ์ด์์ ์ธ ๊ตฌ์กฐ
- ์์ฐจ์  ๋๋ ๋ฑ๊ธ๋ณ ๋ฐ์ดํฐ ์ ๊ทผ์ ์ด์์ 

    ๐ก ๋น์ถ! ์ด์ง ํธ๋ฆฌ์ ์ด์ง ํ์ ํธ๋ฆฌ์ ์ฐจ์ด์ 
        <br>
      - **์ด์ง ํธ๋ฆฌ**    
            : ๋ธ๋์ ์ต๋ Branch๊ฐ 2์ธ ํธ๋ฆฌ
        <br>
      - **์ด์ง ํ์ ํธ๋ฆฌ (Binary Search Tree, BST)**
            : ์ด์ง ํธ๋ฆฌ์ ์ถ๊ฐ์ ์ธ ์กฐ๊ฑด์ด ์๋ ํธ๋ฆฌ
                <br>
            โ ์กฐ๊ฑด : ์ผ์ชฝ ๋ธ๋๋ ํด๋น ๋ธ๋๋ณด๋ค ์์ ๊ฐ, ์ค๋ฅธ์ชฝ ๋ธ๋๋ ํด๋น ๋ธ๋๋ณด๋ค ํฐ ๊ฐ์ ๊ฐ์ง๊ณ  ์์.
    {.:notice}    

<br>

# **Binary Search Tree**

> ์ด์ง ํ์ ํธ๋ฆฌ
> 

![Untitled](https://user-images.githubusercontent.com/100582309/164373662-537c4770-d1ea-4569-b0e9-808aa0152b1e.png)

์ด์ง ํ์ ํธ๋ฆฌ๋ ํธ๋ฆฌ๊ฐ ๋น์ด์์ง ์์ ๊ฒฝ์ฐ ๋ค์๊ณผ ๊ฐ์ ์์ฑ์ ๋ง์กฑํ๋ค.

- ๋ชจ๋  ๋ธ๋ x์ ๋ํ์ฌ,
    
    **์ผ์ชฝ ์๋ธํธ๋ฆฌ**์ ๋ชจ๋  key๊ฐ์ ๋ธ๋ **x์ key๊ฐ๋ณด๋ค ์๋ค**.
    
    **์ค๋ฅธ์ชฝ ์๋ธํธ๋ฆฌ**์ ๋ชจ๋  key๊ฐ์ ๋ธ๋ **x์ key๊ฐ๋ณด๋ค ํฌ๋ค**.
    
- ๋ชจ๋  ๋ธ๋๋ **์๋ก ๋ค๋ฅธ ์ ์ผํ key๊ฐ**์ ๊ฐ์ง
- ์ผ์ชฝ ์๋ธ ํธ๋ฆฌ์ ์ค๋ฅธ์ชฝ ์๋ธ ํธ๋ฆฌ๋ ์ด์ง ํ์ ํธ๋ฆฌ์
- ์ด์ง ํ์ ํธ๋ฆฌ๋ฅผ **์ค์ ์ํ**ํ๋ฉด **์ค๋ฆ์ฐจ์**์ผ๋ก ์ ๋ ฌ๋ ๊ฐ์ ์ป์ ์ ์์


<br>

## **the class `BinarySearchTree`**

---

์์์ด ์ํ๋จ์ ๋ฐ๋ผ ์ด์ง ํ์ ํธ๋ฆฌ์ ๋ชจ์๊ณผ ์์๋ค์ ์๊ฐ ๋ณํ๊ธฐ ๋๋ฌธ์, ์ผ๋ฐ์ ์ผ๋ก linked representation์ ์ฌ์ฉํ์ฌ ํํ๋๋ค.

<details>
    <summary>Python ๊ตฌํ</summary>
    ํด๋์ค ์ ์ ๋ฐ ์ด๊ธฐํ
    <div markdown="1">
    
    class Node(object):                       # ๋จผ์ ย Nodeย ํด๋์ค๋ฅผ ์ ์
        def __init__(self, data):
            self.data = data
            self.left = self.right = None     # ์ด๊ธฐํํ  ๋๋ ๋ฐ์ดํฐ ๊ฐ๋ง ์ฃผ์ด์ง๊ณ  ์ข์ฐ ๋ธ๋๋ ๋น์ด์์
    
    class BinarySearchTree(object):
        def __init__(self):
            self.root = None                # ์ฒ์์๋ ๋น์ด ์๋ ํธ๋ฆฌ๋ก ์ด๊ธฐํ
</div>
</details>

<br>

### **`Ascend()`**

> ๋ชจ๋  ์์๋ฅผ key์ ์ค๋ฆ์ฐจ์์ผ๋ก ์ถ๋ ฅ
> 

<br>

### **`Find(key)`**

> ํ์
> 

---

๋ฃจํธ๋ถํฐ ์์ํ๋ฉฐ, key๊ฐ ๋ฃจํธ๋ณด๋ค ์์ผ๋ฉด ์ผ์ชฝ ํ์ ํธ๋ฆฌ๊ฐ ํ์๋๊ณ  key๊ฐ ๋ฃจํธ๋ณด๋ค ํฌ๋ฉด ์ค๋ฅธ์ชฝ ํ์ ํธ๋ฆฌ๊ฐ ํ์๋๋ค. key๊ฐ ๋ฃจํธ์ ๊ฐ์ผ๋ฉด ๊ฒ์์ด ์ฑ๊ณต์ ์ผ๋ก ์ข๋ฃ๋๋ค.

- ๋ฃจํธ๊ฐ NULL์ด๋ฉด ํ์ ํธ๋ฆฌ๊ฐ ํธ๋ฆฌ๊ฐ ๋น์ด ์์ด ํ์ ์คํจ
- ์๊ฐ ๋ณต์ก๋ O(height)
<details>
    <summary>Python ๊ตฌํ : find() Method</summary>
    ์ฌ๊ท์ ๊ฐ์ ๋์๊ด๊ณ ๋น๊ต๋ฅผ ํตํด ๊ตฌํํ  ์ ์๋ค.
    <div markdown="1">
    
    class BinarySearchTree(object):
        ...
        def find(self, key):
            return self._find_value(self.root, key)
        def _find_value(self, root, key):
            if root is None or root.data == key:
                return root is not None
            elif key < root.data:
                return self._find_value(root.left, key)
            else:
                return self._find_value(root.right, key)
</div>
</details>

<br>

### **`Insert(key)`**

> ์ฝ์
> 

---

- ์ด์ง ๊ฒ์ ํธ๋ฆฌ์ ์ ์์ e๋ฅผ ์ฝ์ํ๋ ค๋ฉด ๋จผ์  ํธ๋ฆฌ์์ ํ์์ ์ํํ์ฌ key๊ฐ ์ด๋ฏธ ์กด์ฌํ์ง ์๋์ง ํ์ธํด์ผ ํ๋ค.
- ํ์์ด ์ฑ๊ณตํ๋ฉด ์ฝ์ํ์ง ์์ผ๋ฉฐ, ํ์์ ์คํจํ๋ฉด ์์๊ฐ ๊ฒ์์ด ์ข๋ฃ๋ ์ง์ ์ ์ฝ์๋๋ค.
    
๐ก ์ ๊ทธ ์ง์ ์ ์ฝ์๋๋๊ฐ?
<br>
ํ์์ ์๋ฆฌ๋ฅผ ์ดํดํ์ผ๋ฉด ์ฝ๋ค. ํ์ ์ค ๋ฃจํธ๊ฐ NULL์ผ๋ก ํธ๋ฆฌ๊ฐ ๋น์ด์๋ ๊ฒฝ์ฐ ํ์์ ์คํจํ๊ธฐ ๋๋ฌธ{: .notice}
    
- ์๊ฐ ๋ณต์ก๋ O(height)
- ex: insert key=7
    
    ![Untitled 1](https://user-images.githubusercontent.com/100582309/164374876-e33e8848-e06a-4dbe-82a5-31bde948428e.png)
    
    <details>
        <summary>Python ๊ตฌํ : Insert Method</summary>
        ์ฌ๊ท๋ฅผ ์ด์ฉํด์ ๊ตฌํํ๋ฉด ๊ฐ๋จํ๋ค. ์๋ก ์ถ๊ฐํ  ์์์ ๊ฐ์ ํ์ฌ ๋ธ๋์ ๊ฐ๊ณผ ๋น๊ตํ์ฌ ์ผ์ชฝ/์ค๋ฅธ์ชฝ ์ค ์๋ง์ ์์น๋ก ๋ธ๋๋ฅผ ์ฎ๊ฒจ๊ฐ๋ฉด์ ์ฝ์ ์์น๋ฅผ ํ์ธํ๋ค.
        <div markdown="1">
        
        class BinarySearchTree(object):
            ...
            def insert(self, data):
                self.root = self._insert_value(self.root, data)
                return self.root is not None
            def _insert_value(self, node, data):
                if node is None:
                    node = Node(data)
                else:
                    if data <= node.data:
                        node.left = self._insert_value(node.left, data)
                    else:
                        node.right = self._insert_value(node.right, data)
                return node
    </div>
    </details>

<br>

### **`Delete(key)`**

> ์ญ์ 
> 

---

์์์ ์ญ์ ๋ ์ธ ๊ฐ์ง ๊ฒฝ์ฐ๋ก ๋๋์ด ์๊ฐํด๋ณผ ์ ์๋ค.

- **case 1** : ์์๊ฐ leaf์ ์๋ค.
    - delete key=7
        
        ![Untitled 2](https://user-images.githubusercontent.com/100582309/164374880-e3b3ac2f-b32a-4a1a-8c7e-9b76e22867a0.png)

        
    <br>
    

- **case 2** : ์์๊ฐ ์ฐจ์ 1์ ๋ธ๋์ ์๋ค(์ฆ, ๋น์ด ์์ง ์์ ์๋ธํธ๋ฆฌ๊ฐ ํ๋ ์กด์ฌ).
    - **delete key=40**
        ![Untitled 3](https://user-images.githubusercontent.com/100582309/164374882-04af48c6-a4fa-4e2e-90a6-06e4a641259e.png)

    <br>
    
    - **delete key=15**
        ![Untitled 4](https://user-images.githubusercontent.com/100582309/164374884-0bba8215-56eb-4fc6-98d5-056719c2ea29.png)

    
    <br>
        

- **case 3** : ์์๊ฐ ์ฐจ์ 2์ ๋ธ๋์ ์๋ค (์ฆ, ๋น์ด ์์ง ์์ ๋ ๊ฐ์ ์๋ธํธ๋ฆฌ๊ฐ ์กด์ฌ).
    - ex 1) delete key=10s
        | ![Untitled 5](https://user-images.githubusercontent.com/100582309/164374888-ca9c9854-4be8-4db0-bc79-165828fcf0a9.png) | 
        |:--:| 
        |step 1|  
        <br>
        | ![Untitled 6](https://user-images.githubusercontent.com/100582309/164374890-2045f033-f222-4fd0-b454-9349b16df789.png) | 
        |:--:| 
        |step 2. ์ผ์ชฝ ์๋ธํธ๋ฆฌ์์ ๊ฐ์ฅ ํฐ key(๋๋ ์ค๋ฅธ์ชฝ ํ์ ํธ๋ฆฌ์์ ๊ฐ์ฅ ์์ key)๋ก ๋์ฒด|
        <br>
        | ![Untitled 7](https://user-images.githubusercontent.com/100582309/164374893-5ec236d1-e7f0-4ac6-bef3-4e1042a04722.png) | 
        |:--:| 
        |step 3. ๊ฐ์ฅ ํฐ key๋ leaf ๋๋ ์ฐจ์ 1์ธ ๋ธ๋์ ์์ด์ผ ํ๋ค.|

    <br>    

    - ex 2) delete key=20

        | ![Untitled 8](https://user-images.githubusercontent.com/100582309/164374895-d2ec36cb-b529-49fe-8427-50413f1e1430.png) | 
        |:--:| 
        |์ผ์ชฝ ์๋ธํธ๋ฆฌ์ ๊ฐ์ฅ ํฐ key๊ฐ์ผ๋ก ๋์ฒดํ๋ค.|
        

- ์ผ์ชฝ ์๋ธ ํธ๋ฆฌ์์ key๊ฐ ๊ฐ์ฅ ํฐ ๋ธ๋(+ ์ค๋ฅธ์ชฝ ์๋ธ ํธ๋ฆฌ์์ key๊ฐ ๊ฐ์ฅ ์์ ๋ธ๋)๋ 0 ๋๋ ๋น์ด ์์ง ์์ ์๋ธ ํธ๋ฆฌ๊ฐ ํ๋ ์๋ ๋ธ๋์ ์์ด์ผ ํฉ๋๋ค.

<BR>

**๐ก ๋ธ๋์ ์ผ์ชฝ ์๋ธํธ๋ฆฌ์์ key๊ฐ ๊ฐ์ฅ ํฐ ๋ธ๋๋ฅผ ์ฐพ๋ ๋ฐฉ๋ฒ**
<br>
์๋ธ ํธ๋ฆฌ์ ๋ฃจํธ๋ก ์ด๋ํ ๋ค์ ์ค๋ฅธ์ชฝ ์์์ ํฌ์ธํฐ๊ฐ NULL์ธ ๋ธ๋์ ๋๋ฌํ  ๋๊น์ง ๊ณ์ ์ค๋ฅธ์ชฝ ์์ ํฌ์ธํฐ๋ฅผ ๋ฐ๋ผ๊ฐ๋ค.
{: .notice}

**๐ก ๋ธ๋์ ์ค๋ฅธ์ชฝ ์๋ธํธ๋ฆฌ์์ key๊ฐ ๊ฐ์ฅ ์์ ๋ธ๋๋ฅผ ์ฐพ๋ ๋ฐฉ๋ฒ**
<br>
์๋ธ ํธ๋ฆฌ์ ๋ฃจํธ๋ก ์ด๋ํ ๋ค์ ์ผ์ชฝ ์์์ ํฌ์ธํฐ๊ฐ NULL์ธ ๋ธ๋์ ๋๋ฌํ  ๋๊น์ง ๊ณ์ ์ผ์ชฝ ์์ ํฌ์ธํฐ๋ฅผ ๋ฐ๋ผ๊ฐ๋ค.
{: .notice}
<br>

- ์๊ฐ๋ณต์ก๋ : O(height)
<details>
    <summary>Python ๊ตฌํ : delete() method</summary>
    <div markdown="1">

        class BinarySearchTree(object):
            ...
            def delete(self, key):
                self.root, deleted = self._delete_value(self.root, key)
                return deleted
            def _delete_value(self, node, key):
                if node is None:
                    return node, False
                deleted = False
                if key == node.data:
                    deleted = True
                    if node.left and node.right:
                                        # replace the node to the leftmost of node.right
                        parent, child = node, node.right
                        while child.left is not None:
                            parent, child = child, child.left
                        child.left = node.left
                        if parent != node:
                            parent.left = child.right
                            child.right = node.right
                        node = child
                    elif node.left or node.right:
                        node = node.left or node.right
                    else:
                        node = None
                elif key < node.data:
                    node.left, deleted = self._delete_value(node.left, key)
                else:
                    node.right, deleted = self._delete_value(node.right, key)
                return node, deleted

</div>
</details>

<br>

## Binary Search Tree์ ์๊ฐ๋ณต์ก๋

---

- ์ด์ง ํธ๋ฆฌ์ ์ข์ฐ ๊ท ํ์ด ๋ง๋๋ค๋ฉด ํ์, ์ฝ์, ์ญ์ ์ ์๊ฐ๋ณต์ก๋๊ฐย `O(log n)`
- ๊ทธ๋ฌ๋ ์ด์ง ํ์ ํธ๋ฆฌ๋ ์ ๋ ฌ๋ ๋ฐ์ดํฐ์๋ ์ทจ์ฝํ๋ค.
    
    โ ์ค๋ฆ์ฐจ์์ด๋  ๋ด๋ฆผ์ฐจ์์ด๋  ์ ๋ ฌ๋ ๋ฐ์ดํฐ๊ฐ ์๋ ฅ๋๋ฉด ํธํฅ ํธ๋ฆฌ(`skewed tree`)๊ฐ ์๊น
    
    โ ์ต์์ ๊ฒฝ์ฐ ๋ชจ๋  ๋ฐ์ดํฐ๋ฅผ ์ดํด์ผ ํ  ์๋ ์์ด ์๊ฐ๋ณต์ก๋๊ฐย `O(n)`์ด ๋๋ค.
    

<br><br>

# **Indexed Binary Search Trees**

---

-

![Untitled 9](https://user-images.githubusercontent.com/100582309/164374896-3409ac0c-fab3-440b-a525-bfa89efa6d85.png)


- ๊ฐ ๋ธ๋์๋ ์ถ๊ฐ์ ์ธ ํ๋ โLeftSizeโ๋ผ๋ ๋ณ์๊ฐ ์์
- ์ด์ง ํ์ ํธ๋ฆฌ์ ๊ธฐ๋ฅ ๋ฟ๋ง ์๋๋ผ ์์(rank)๋ณ ๊ฒ์ ๋ฐ ์ญ์  ์์์ด ๊ฐ๋ฅ
- LeftSize : ์ผ์ชฝ ์๋ธํธ๋ฆฌ์ ์์ ์

<br>

### **LeftSize์ Rank**

- ์์์ rank ์ฆ, ์์๋ ์ค๋ฆ์ฐจ์ ์์์ ๋ฐ๋ฅธ ์์น๋ฅผ ๊ฐ๋ฆฌํจ๋ค.
    
    [2, 6, 7, 8, 10, 15, 18, 20, 25, 30, 35, 40]
    
    rank(2) = 0
    
    rank(15) = 5
    
    rank(20) = 7
    
    ![Untitled 10](https://user-images.githubusercontent.com/100582309/164374899-db6866bd-ce3e-4050-a865-3ac2e82037ad.png)

    
- x๋ฅผ ๋ฃจํธ๋ธ๋๋ก ํ๋ ์๋ธํธ๋ฆฌ์ ์์์ ๋ํ์ฌ **LeftSize(x) = rank(x)**
- indexed Binary Tree์ ํ์ฉ
    
    `insert(index, element)`
    

    ![Untitled 11](https://user-images.githubusercontent.com/100582309/164374900-b1589997-8955-43e9-a743-668a0f1e3f87.png)


    ![Untitled 12](https://user-images.githubusercontent.com/100582309/164374903-eb85d21c-e47e-424a-ad53-f3cf2ec4b50c.png)


    ![Untitled 13](https://user-images.githubusercontent.com/100582309/164374904-c5000753-2bfd-4ab0-b082-14cdb7256d0a.png)


    ![Untitled 14](https://user-images.githubusercontent.com/100582309/164374907-fe8095d6-e884-4aa9-b2b8-40c225d8af50.png)

- ์ฌ๋ฌ ๊ฐ๋ฅ์ฑ์ด ์กด์ฌํ  ์ ์๋ค.
- ๋ฃจํธ ๋ธ๋๋ถํฐ ์๋ก์ด ๋ธ๋๊น์ง์ leftSize๋ฅผ ์๋ฐ์ดํธํด์ผ ํ๋ค.
- ์๊ฐ ๋ณต์ก๋๋ O(height)

<br><br>

## **Binary Search Tree with Duplicates**

์ค๋ณต์ด ์๋ ์ด์ง ํ์ ํธ๋ฆฌ

---

์ด์ง ํ์ ํธ๋ฆฌ์ ๋ชจ๋  ์์์ ๋ณ๋์ key๊ฐ ํ์ํ๋ค๋ ์๊ตฌ์ฌํญ์ ์ ๊ฑฐํด๋ณผ ์ ์๋ค.

For every node x,

all keys in the left subtree of x are  **smaller** than that in x.
all keys in the right subtree of x are **larger** than that in x

- โsmallerโ โ "smaller or equal to"๋ก,
- "larger" โ "larger or equal to"์ผ๋ก ๋ฐ๊พผ๋ค.

๊ทธ๋ฌ๋ฉด ์ด์ง ๊ฒ์ ํธ๋ฆฌ๊ฐ ์ค๋ณต ํค๋ฅผ ๊ฐ์ง ์ ์๊ฒ ๋๋ค.

<br>
<br>

### ์ฐธ๊ณ ์๋ฃ
- [https://geonlee.tistory.com/72](https://geonlee.tistory.com/72)
