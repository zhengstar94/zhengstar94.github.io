---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "226.Invert Binary Tree"
date: "2024-03-01"
categories:
  - "LeetCode Trees"
---

# LeetCode 226. Invert Binary Tree 

- Given the `root` of a binary tree, invert the tree, and return *its root*.

**Example 1**

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

**Example 2**

```
Input: root = [2,1,3]
Output: [2,3,1]
```

**Example 3**

```
Input: root = []
Output: []
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Trees;

/**
 * @author zhengstars
 * @date 2024/03/31
 */
public class InvertBinaryTree {

    // Function to invert a binary tree
    public static  TreeNode invertTree(TreeNode root) {
        // Base case: if the tree is empty, return null
        if(root == null){
            return null;
        }

        // Swap the left and right child of each node
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;

        // Recursively invert the left and right subtree
        invertTree(root.left);
        invertTree(root.right);

        // Return the root of the inverted tree
        return root;
    }

    public static void main(String[] args) {
        // Construct a binary tree
        TreeNode root1 = new TreeNode(4);
        root1.left = new TreeNode(2);
        root1.right = new TreeNode(7);
        root1.left.left = new TreeNode(1);
        root1.left.right = new TreeNode(3);
        root1.right.left = new TreeNode(6);
        root1.right.right = new TreeNode(9);

        // Invert the binary tree
        TreeNode invertedRoot1 = invertTree(root1);
    }
}
```

