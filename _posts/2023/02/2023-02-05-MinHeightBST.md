---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Min Height BST"
date: "2023-02-05"
categories:
  - "Algorithm BST"
---

# Min Height BST [Medium]

- Write a function that takes in a non-empty sorted array of distinct integers, constructs a BST from the integers, and returns the root of the BST.
- The function should minimize the height of the BST.
- You've been provided with a BST class that you'll have to use to construct the BST.
- Each BST node has an integer value, a left child node, and a right child node. A node is said to be a valid BST node if and only if it satisfies the BST property: its value is strictly greater than the values of every node to its left; its value is less than or equal to the values of every node to its right; and its children nodes are either valid BST nodes themselves or None / null.
- A BST is valid if and only if all of its nodes are valid BST nodes.
- Note that the BST class already has an insert method which you can use if you want.

**Sample Input **

```\
array = [1, 2, 5, 7, 10, 13, 14, 15, 22]
```

**Sample Result **

```
         10
       /     \
      2      14
    /   \   /   \
   1     5 13   15
          \       \
           7      22
// This is one example of a BST with min height
// that you could create from the input array.
// You could create other BSTs with min height
// from the same array; for example:
         10
       /     \
      5      15
    /   \   /   \
   2     7 13   22
 /           \
1            14
```

**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> In order for the BST to have the smallest height possible,it needs to be balanced;in other words,it needs to have roughly the same number of nodes in its left subtree as in its right subtree. </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> How can you use the sorted nature of the input array to construct a balanced BST?  </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Grab the middle element of the array,and make that element be the root node of the BST.
Then,grab the middle element between the beginning of the array and the first middle element,and make that element be the root of the BST's left subtree;similarly,make the middle element between the end of the array and the first middle element be the root of the BST's right subtree.Continue this approach until you run out of elements in the array.  </strong></i>
</details>

<br>

## Method 1(insert)

```tex
【O(nlog(n))time∣O(n)space】
```

```java
package NewArray;

/**
 * @author zhengstars
 * @date 2023/01/10
 */
public class MinHeightBST {

    static class BST {
        public int value;
        public BST left;
        public BST right;

        public BST(int value) {
            this.value = value;
        }

        BST(int value, BST left, BST right) {
            this.value = value;
            this.left = left;
            this.right = right;
        }
    }

    public static void insert(BST tree, int value){
        if(value < tree.value){
            if(null == tree.left){
                tree.left = new BST(value);
            }else{
                insert(tree.left,value);
            }
        }else{
            if(null == tree.right){
                tree.right = new BST(value);
            }else{
                insert(tree.right,value);
            }
        }
    }

    public static BST minHeightBst(int[] array){
        BST bst = null;
        return constructMinHeightBst(array,bst,0,array.length - 1);
    }

    private static BST constructMinHeightBst(int[] array, BST bst, int left, int right) {
        if(right < left){
            return null;
        }
        int mid = left + (right - left)/2;
        int midNode = array[mid];
        if(null == bst){
            bst = new BST(midNode);
        }else{
            insert(bst,midNode);
        }
        constructMinHeightBst(array,bst,left,mid - 1);
        constructMinHeightBst(array,bst,mid + 1,right);
        return bst;
    }

    public static void main(String[] args) {
        int[] array = new int[]{1,2,5,7,10,13,14,15,22};
        BST bst = minHeightBst(array);
        System.out.println(bst);
    }
}

```



## Method 2()

```tex
【O(nlog(n))time∣O(1)space】
```

```java
package NewArray;

/**
 * @author zhengstars
 * @date 2023/01/10
 */
public class MinHeightBST {

    static class BST {
        public int value;
        public BST left;
        public BST right;

        public BST(int value) {
            this.value = value;
        }

        BST(int value, BST left, BST right) {
            this.value = value;
            this.left = left;
            this.right = right;
        }
    }


    public static BST minHeightBst(int[] array){
        BST bst = null;
        return constructMinHeightBst(array,bst,0,array.length - 1);
    }

    private static BST constructMinHeightBst(int[] array, BST bst, int left, int right) {
        if(right < left){
            return null;
        }
        int mid = left + (right - left)/2;
        BST newBst = new BST(array[mid]);

        if(null == bst){
            bst = newBst ;
        }else{
            if(array[mid] < bst.value){
                bst.left = newBst;
                bst = bst.left;
            }else{
                bst.right = newBst;
                bst = bst.right;
            }
        }

        constructMinHeightBst(array,bst,left,mid - 1);
        constructMinHeightBst(array,bst,mid + 1,right);
        return bst;
    }

    public static void main(String[] args) {
        int[] array = new int[]{1,2,5,7,10,13,14,15,22};
        BST bst = minHeightBst(array);
        System.out.println(bst);
    }
}

```

## Method 3

```tex
【O(n)time∣O(n)space】
```

```java
package NewArray;

/**
 * @author zhengstars
 * @date 2023/01/10
 */
public class MinHeightBST {

    static class BST {
        public int value;
        public BST left;
        public BST right;

        public BST(int value) {
            this.value = value;
        }

        BST(int value, BST left, BST right) {
            this.value = value;
            this.left = left;
            this.right = right;
        }
    }

    public static BST minHeightBst(int[] array){
        return constructMinHeightBst(array,0,array.length - 1);
    }

    private static BST constructMinHeightBst(int[] array,  int left, int right) {
        if(right < left){
            return null;
        }
        int mid = left + (right - left)/2;

        BST bst = new BST(array[mid]);

        bst.left = constructMinHeightBst(array,left,mid - 1);
        bst.right = constructMinHeightBst(array,mid + 1,right);
        return bst;
    }

    public static void main(String[] args) {
        int[] array = new int[]{1,2,5,7,10,13,14,15,22};
        BST bst = minHeightBst(array);
        System.out.println(bst);
    }
}

```


