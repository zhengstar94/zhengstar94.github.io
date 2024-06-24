---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "98.Validate Binary Search Tree"
date: "2024-03-07"
categories:
  - "LeetCode Trees"
---

# LeetCode 98. Validate Binary Search Tree

- Given the `root` of a binary tree, *determine if it is a valid binary search tree (BST)*.

  A **valid BST** is defined as follows:

  - The left subtree of a node contains only nodes with keys **less than** the node's key.
  - The right subtree of a node contains only nodes with keys **greater than** the node's key.
  - Both the left and right subtrees must also be binary search trees.

**Example 1**

```
Input: root = [2,1,3]
Output: true
```

**Example 2**

```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

## Method 1

```tex
【O(n)time∣O(h)space】
```

```java
package Leetcode.Trees;

/**
 * This class is used to verify if a binary tree is a binary search tree.
 * @author zhengstars
 * @date 2024/04/09
 */
public class ValidateBinarySearchTree {
    // Declare a global variable which will always store the value of the last accessed node.
    static long pre = Long.MIN_VALUE;

    public static boolean isValidBST(TreeNode root) {
        // An empty tree is a binary search tree
        if (root == null) {
            return true;
        }

        // Validate the left subtree
        if (!isValidBST(root.left)) {
            return false;
        }

        // Validate current node
        if (root.val <= pre) {
            return false;
        }
        pre = root.val;

        // Validate the right subtree
        return isValidBST(root.right);
    }

    public static void main(String[] args) {

        // Create the first test case
        TreeNode root1 = new TreeNode(2);
        root1.left = new TreeNode(1);
        root1.right = new TreeNode(3);
        // The expected output is true
        System.out.println(isValidBST(root1));

        // Create the second test case
        TreeNode root2 = new TreeNode(5);
        root2.left = new TreeNode(1);
        root2.right = new TreeNode(4);
        root2.right.left = new TreeNode(3);
        root2.right.right = new TreeNode(6);
        // The expected output is false
        System.out.println(isValidBST(root2));

        // Create the third test case: an empty tree, the expected output is true
        System.out.println(isValidBST(null));
    }
}
```

