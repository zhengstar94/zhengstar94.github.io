---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Node Depths"
date: "2023-03-02"
categories:
  - "Algorithm Binary Trees"
---

# Node Depths [Easy]

- The distance between a node in a Binary Tree and the tree's root is called the node's depth.

- Write a function that takes in a Binary Tree and returns the sum of its nodes'depths.

- Each `BinaryTree` node has an integer `value`,  a `left` child node, and a `right` child node. Children nodes can either be `BinaryTree` nodes themselves or `None`/`null`.





**Sample Input **

````
true =    1
       /     \
      2       3
    /   \    /  \
   4     5  6     7
 /   \   
2     9  
````

**Sample Output **

```
16
//The depth of the node with value 2 is 1. 
//The depth of the node with value 3 is 1.
//The depth of the node with value 4 is 2. 
//The depth of the node with value 5 is 2. 
//Etc..
//Summing all of these depths yields 16.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> As obvious as it may seem, to solve this question, you'll have to figure out how to compute the depth of any given node; once you know how to do that,you can compute all of the depths and add them up to obtain the desired output. </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> To compute the depth of a given node,you need information about its position in the tree. Can you pass this information down from the node's parent? </strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> The depth of any node in the tree is equal to the depth of its parent node plus 1. By starting at the root node whose depth is 0, you can pass down to every node in the tree its respective depth, and you can implement the algorithm that does this and that sums up all of the depths either recursively or iteratively.</strong></i>
</details>




<br>



## Method 1

```tex
【O(n)time∣O(h)space】
```

```java
package BT;

import com.sun.source.tree.BinaryTree;

/**
 * @author zhengstars
 * @date 2023/02/08
 */
public class NodeDepths {

    static class BST {
        public int value;
        public BST left;
        public BST right;
        BST() {}
        public BST(int value) {
            this.value = value;
        }
    }

    public static int nodeDepths(BST root) {
        return helper(root,0);
    }

    private static int helper(BST node, int depth) {
        if (node == null) {
            return 0;
        }

        return depth + helper(node.left, depth + 1) + helper(node.right, depth + 1);
    }
}

```





