---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Height Balanced Binary Tree"
date: "2023-03-07"
categories:
  - "Algorithm Binary Trees"
---

# Height Balanced Binary Tree [Medium]

- You're given the root node of a Binary Tree. Write a function that returns `true` if this Binary
  Tree is height balanced and `false` if it isn't.

- A Binary Tree is height balanced if for each node in the tree, the difference between the height of its left subtree and the height of its right subtree is at most `1`.

- Each `BinaryTree` node has an integer `value`, a `left` child node, and a `right` child node. Children nodes can either be `BinaryTree` nodes themselves or `None`/` null`.







**Sample Input **

````
true =       1
          /    \
         2        3
       /   \        \
      4     5        6
          /   \       
        7	      8
   
````

**Sample Output **

```
true
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> To solve this problem, you'll have to determine if every subtree in the Binary Tree is balanced. Which subtrees do you know will always be balanced? </strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> To determine if a subtree is balanced,you need to know the height of its left and right subtrees. The only exception to this is if a subtree has no left and right subtrees (i.e., it's just a leaf node); in that case, the subtree must be balanced.</strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Recursively calculate the left and right subtree heights from each node. Once you know the heights of a particular node's left and right subtrees, you can determine if the subtree rooted at that node is balanced. If a subtree ever isn't balanced, you can immediately conclude that the entire tree isn't balanced. If you make it through the entire tree without finding any unbalanced subtrees, and if you determine that the heights of the main two subtrees aren't more than 1 apart, then the entire tree is balanced. </strong></i>
</details>

<br>



## Method 1

```tex
【O(nlog(n))time∣O(h)space】
```

```java
package BT;

/**
 * @author zhengstars
 * @date 2023/02/24
 */
public class HeightBalancedBinaryTree {

    static class BinaryTree {
        public int value;
        public BinaryTree left = null;
        public BinaryTree right = null;
        public BinaryTree parent = null;

        public BinaryTree(int value) {
            this.value = value;
        }
    }


    public boolean heightBalancedBinaryTree(BinaryTree root) {
        // if the root is null, return true
        if(null == root){
            return true;
        }

        // calculate the heights of left and right subtrees
        int leftHight = getHeight(root.left);
        int rightHight = getHeight(root.right);

        // check if the height difference between the left and right subtrees is at most 1
        // and recursively check if the left and right subtrees are also height balanced
        return (Math.abs(leftHight - rightHight) <= 1) && heightBalancedBinaryTree(root.left) &&  heightBalancedBinaryTree(root.right);
    }

    private int getHeight(BinaryTree root) {
        // if the root is null, return 0
        if(null == root){
            return 0;
        }
        // calculate the height of the left and right subtrees and return the maximum of the two plus 1
        return Math.max(getHeight(root.left),getHeight(root.right))+1;
    }
}

```



## Method 2

```tex
【O(n)time∣O(h)space】
```

```java
package BT;

/**
 * @author zhengstars
 * @date 2023/02/24
 */
public class HeightBalancedBinaryTree {

  static class BinaryTree {
    public int value;
    public BinaryTree left = null;
    public BinaryTree right = null;
    public BinaryTree parent = null;

    public BinaryTree(int value) {
      this.value = value;
    }
  }

  private boolean balanced = true;

  public boolean heightBalancedBinaryTree2(BinaryTree root) {
    // reset the balanced flag to true for each recursive call
    balanced = true;
    // calculate the height of the tree rooted at the given BinaryTree and check if it is balanced
    height(root);
    // return the value of the balanced flag after checking the entire tree
    return balanced;
  }

  private int height(BinaryTree root) {
    // if the current BinaryTree is null or the tree is not height-balanced, return -1
    if (null == root || !balanced) {
      return -1;
    }

    // calculate the height of the left and right subtrees of the current BinaryTree recursively
    int left = height(root.left);
    int right = height(root.right);

    // check if the difference between the heights of the left and right subtrees is greater than 1
    if (Math.abs(left - right) > 1) {
      balanced = false;
      return -1;
    }

    // return the height of the current BinaryTree, which is the maximum height of its left and right subtrees plus 1
    return Math.max(left, right) + 1;
  }
}
```





