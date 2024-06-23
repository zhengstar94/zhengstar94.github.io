---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Invert Binary Tree"
date: "2023-03-04"
categories:
  - "Algorithm Binary Trees"
---

# Invert Binary Tree [Medium]

- Write a function that takes in a Binary Tree and inverts it. In other words, the function should swap every left node in the tree for its corresponding right node.

- Each `BinaryTree` node has an integer `value`, a `left` child node, and a `right` child node. Children nodes can either be `BinaryTree` nodes themselves or `None`/`null`.





**Sample Input **

````
true =     1
        /      \
      2         3
    /    \     /   \
   4      5   6      7
 /   \   
8     9  
````

**Sample Output **

```
true =     1
        /      \
      3          2
    /    \     /   \
   7      6   5      4
                   /   \   
                  9     8 
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Start by inverting the root node of the Binary Tree. Inverting this root node simply consists of swapping its left and right child nodes, which can be done the same way as swapping two variables. </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Once the first swap mentioned in Hint #1 is done, you must invert the root node's left child node and its right child node. You can do so just as you did for the root node.Then, you will have to continue inverting child nodes'nodes until you reach the bottom of the tree.</strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> How can you accomplish the process described in Hint #2? While recursion seems appropriate, would an iterative approach work?What would be the time and space complexity implications of both approaches?</strong></i>
</details>



<br>



## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package BT;

/**
 * @author zhengstars
 * @date 2023/02/11
 */
public class InvertBinaryTree {

    static class BinaryTree {
        public int value;
        public BinaryTree left;
        public BinaryTree right;

        public BinaryTree(int value) {
            this.value = value;
        }
    }

    public static void invertBinaryTree(BinaryTree tree) {
        if(null == tree){
            return;
        }

        BinaryTree left = tree.left;
        BinaryTree right = tree.right;

        tree.left = right;
        tree.right = left;

        invertBinaryTree(tree.left);
        invertBinaryTree(tree.right);
    }
}

```





