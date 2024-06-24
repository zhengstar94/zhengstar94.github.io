---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "104.Maximum Depth of Binary Tree"
date: "2024-03-02"
categories:
  - "LeetCode Trees"
---

# LeetCode 104. Maximum Depth of Binary Tree 

- Given the `root` of a binary tree, return *its maximum depth*.
- A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1**

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

**Example 2**

```
Input: root = [1,null,2]
Output: 2
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Trees;

/**
 * @author zhengstars
 * @date 2024/04/04
 */
public class MaximumDepthOfBinaryTree {
    public static int maxDepth(TreeNode root) {
        // if tree is empty, depth is 0
        if (root == null) {
            return 0;
        } else {
            // calculate maximum depth of left subtree
            int leftHeight = maxDepth(root.left);
            // calculate maximum depth of right subtree
            int rightHeight = maxDepth(root.right);
            // tree's maximum depth is maximum of left and right subtree depths plus 1
            return Math.max(leftHeight, rightHeight) + 1;
        }
    }

    public static void main(String[] args) {
        // construct binary tree
        TreeNode root1 = new TreeNode(3);
        root1.left = new TreeNode(9);
        root1.right = new TreeNode(20);
        root1.right.left = new TreeNode(15);
        root1.right.right = new TreeNode(7);

        // find the maximum depth
        int depth1 = maxDepth(root1);
        // output: 3
        System.out.println(depth1);
    }
}
```

