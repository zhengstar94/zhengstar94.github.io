---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Validate Binary Search Tree"
date: "2023-02-03"
categories:
  - "Algorithm BST"
---

# Validate Binary Search Tree [Medium]

- Given a binary tree, determine if it is a valid binary search tree (BST).
- Assume a BST is defined as follows:
  - The left subtree of a node contains only nodes with keys **less than** the node’s key.
  - The right subtree of a node contains only nodes with keys **greater than** the node’s key.
  - Both the left and right subtrees must also be binary search trees.

**Sample Usage #1**

> ```
>      2
>    /   \
>   1     3
> ```

**Sample Output #1**

> Binary tree`[2,1,3]`,return true.

**Sample Usage #1**

> ```
>      1
>    /   \
>   2     3
> ```

**Sample Output #1**

> Binary tree`[1,2,3]`,return false.


**Hints**
<br>
<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Every node in the BST has a maximum possible value and a minimum possible value.In other words,the value of any given node in the BST must be strictly smaller than some value (the value of its closest right parent)and must be greater than or equal to some other value (the value of its closest left parent). </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> Validate the BST by recursively calling the validateBst function on every node,passing in the correct maximum and minimum possible values to each.Initialize those values to be -Infinity and + Infinity. </strong></i>
</details>

<br>


## Method 1

```tex
【 O(n)time | O(n) space 】
```

```java
package NewArray;

/**
 * @author zhengstars
 * @date 2023/01/05
 */
public class ValidateBST {
  public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
      this.val = val;
      this.left = left;
      this.right = right;
    }
  }

  public static boolean isValidBST(TreeNode root) {
    return validBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
  }

  public static boolean validBST(TreeNode root, long min, long max){
    // if root is null, return true
    if(null == root){
      return true;
    }

    // limit the range of each subtree, if root.val smaller than min or bigger than max, return false
    if(min >=  root.val || max <= root.val){
      return false;
    }

    // Recursion this method
    return validBST(root.left,min,root.val) && validBST(root.right,root.val,max);
  }

}
```



## Method 2

```tex
【 O(n)time | O(h) space 】
```



```java
package NewArray;

/**
 * Do an in-order traversal, the numbers should be sorted, thus we only need to compare with the previous number.
 * @author zhengstars
 * @date 2023/01/05
 */
public class ValidateBST2 {
  public static class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
      this.val = val;
      this.left = left;
      this.right = right;
    }
  }

  private static TreeNode prev;
  public static boolean isValidBST(TreeNode root) {
    return inOrder(root);
  }

  private static boolean inOrder(TreeNode root) {
    // End condition: if root is null, return true.
    // Because the empty binary search tree is also qualified.
    if (root == null){
      return true;
    }

    // Recursive left subtree to get the result, if the left subtree does not match then return false
    if (!inOrder(root.left)) {
      return false;
    }

    // If the value of the current node is greater than or equal to the previous value in the middle sequence,
    // it is proved that it is not a binary search tree.
    // If pre is null, it proves that pre has not been initialized, so initialize pre first
    if (prev != null && root.val <= prev.val) {
      return false;
    }

    // If the current node is less than the last value in the middle sequence,
    // it is proved that the condition is met, update pre to the current value,
    // and continue to judge the next value.
    prev = root;

    // Recursive left subtree to get the result,
    // because the left subtree has been judged, only the right subtree needs to be judged.
    return inOrder(root.right);
  }

  public static void main(String[] args) {

    TreeNode bst = new TreeNode(5);
    bst.left = new TreeNode(1);

    TreeNode bst2 = new TreeNode(6);

    bst2.right = new TreeNode(7);
    bst.right = bst2;

    System.out.println(isValidBST(bst));
  }

}

```

