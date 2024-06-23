---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Binary Tree Diameter"
date: "2023-03-05"
categories:
  - "Algorithm Binary Trees"
---

# Binary Tree Diameter [Medium]

- Write a function that takes in a Binary Tree and returns its diameter. The diameter of a binary tree is defined as the length of its longest path, even if that path doesn't pass through the root of the tree.

- A path is a collection of connected nodes in a tree, where no node is connected to more than two other nodes. The length of a path is the number of edges between the path's first node and its last node.

- Each `BinaryTree` node has an integer `value`, a `left` child node, and a `right` child node. Children nodes can either be `BinaryTree` nodes themselves or `None`/ `null`.







**Sample Input **

````
true =        1
          /      \
         3         2
       /   \    
      7     4   
    /         \ 
  8 					  5
 /               \
9                 6
````

**Sample Output **

```
6//9->8->7->3->4->5->6 
//There are 6 edges between the 
//first node and the last node 
//of this tree's longest path.
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> How can you use the height of a binary tree and the heights of its subtrees to calculate its diameter? </strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> The length of the longest path that goes through the root of a binary tree is the sum of the heights of its left and right subtrees (left subtree height right subtree height). The diameter of a binary tree can be calculated by taking the maximum of:1)the maximum subtree diameter (max(left subtree diameter,right subtree diameter)); and 2)the length of the longest path that goes through the root (left subtree height right subtree height).</strong></i>
</details>




<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> Implement a variation of depth-first search that recursively keeps track of both the diameter and the height of a each subtree in the input binary tree. Follow Hint #2 to continuously compute these diameters.</strong></i>
</details>



<br>



## Method 1

```tex
【O(n)time∣O(h)space】
```

```java
import java.util.*;

class Program {
  // This is an input class. Do not edit.
  static class BinaryTree {
    public int value;
    public BinaryTree left = null;
    public BinaryTree right = null;

    public BinaryTree(int value) {
      this.value = value;
    }
  }

  // Maintain global maximum diameter
  private int maxDiamter ;
  public  int binaryTreeDiameter(BinaryTree tree) {
    // Depth-first traversal of binary tree
    helper(tree);
    return maxDiamter;
  }

  // Recursive function that returns the depth of the subtree
  public int helper(BinaryTree tree){
    // If the subtree is empty, return a depth of 0  
    if(null == tree){
      return 0;
    }

    // Recursively call the helper function to the left and right subtrees to get the depth of the left and right subtrees
    int leftSub = helper(tree.left);
    int rightSub = helper(tree.right);

    // Update global maximum diameter
    maxDiamter = Math.max(maxDiamter,leftSub+rightSub);
    
    // Returns the depth of the current subtree (the maximum depth of the left and right subtree plus 1)
    return 1+Math.max(leftSub,rightSub);
  }
}

```



## Method 2

```tex
【O(n)time∣O(h)space】
```

```java
import java.util.*;

class Program {
  // This is an input class. Do not edit.
  static class BinaryTree {
    public int value;
    public BinaryTree left = null;
    public BinaryTree right = null;

    public BinaryTree(int value) {
      this.value = value;
    }
  }

  // The main function that returns the diameter of the binary tree
  public static int binaryTreeDiameter(BinaryTree tree) {
    return binaryTreeDiameterHelper(tree).diameter;
 }

  // Auxiliary function to calculate the height and diameter of the binary tree
  public static HeightAndDiameter binaryTreeDiameterHelper(BinaryTree tree) {
      // if the binary tree is empty, the height and diameter are both 0
      if (tree == null) {
          return new HeightAndDiameter(0, 0);
      }
      
      // recursively calculate the height and diameter of the left and right subtrees, respectively.
      HeightAndDiameter leftResult = binaryTreeDiameterHelper(tree.left);
      HeightAndDiameter rightResult = binaryTreeDiameterHelper(tree.right);

      // calculate the height and diameter of the current node
      int currentHeight = Math.max(leftResult.height, rightResult.height) + 1;
      int currentDiameter = Math.max(leftResult.height + rightResult.height, Math.max(leftResult.diameter, rightResult.diameter));
      return new HeightAndDiameter(currentHeight, currentDiameter);
  }

  // the class used to store the height and diameter of the binary tree
  public static class HeightAndDiameter {
      int height;// the height of the current subtree
      int diameter;// diameter of the current subtree

      public HeightAndDiameter(int height, int diameter) {
          this.height = height;
          this.diameter = diameter;
      }
  }
}

```
