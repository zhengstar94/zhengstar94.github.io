---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "572.Subtree of Another Tree"
date: "2024-03-04"
categories:
  - "LeetCode Trees"
---

# LeetCode 572. Subtree of Another Tree 

- Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of` subRoot` and `false` otherwise.
- A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

**Example 1**

```
Input: root = [3,4,5,1,2], subRoot = [4,1,2]
Output: true
```

**Example 2**

```
Input: root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
Output: false
```

## Method 1

```tex
【O(m*n)time∣O(max(m,d))space】
```

```java
package Leetcode.Trees;

/**
 * @author zhengstars
 * @date 2024/04/06
 */
public class SubtreeOfAnotherTree {
    
    public static boolean isSubtree(TreeNode root, TreeNode subRoot) {
        if(root == null){
            // If root is null, it means it has no subtrees. Hence we return false
            return false;
        }

        // If subRoot is equivalent to root, or if subRoot is a subtree of the left child of root, 
        // or if subRoot is a subtree of the right child of root, we return true.
        return isSameTree(root, subRoot) || isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
        // Detailed explanation:
        // 1. isSameTree(root, subRoot) verifies if root and subRoot are the same. If they are, then subRoot is a subtree of root.
        // 2. isSubtree(root.left, subRoot) checks to see if subRoot is a subtree of the left child of root, in case root and subRoot are not the same.
        // 3. isSubtree(root.right, subRoot) checks to see if subRoot is a subtree of the right child of root, if it wasn't a subtree of the left child.
    }
    
    private static boolean isSameTree(TreeNode root, TreeNode subRoot) {
        if(root == null && subRoot == null){
            // If both trees are empty, then they are equivalent
            return true;
        }

        if(root == null || subRoot == null){
            // If just one of the trees is empty, then they are not equivalent
            return false;
        }

        if(root.val != subRoot.val){
            // If both trees exist but their root values differ, then they are not equivalent
            return false;
        }

        // If the root values are the same, we further need to check if their respective left and right subtrees are equivalent
        return isSameTree(root.left, subRoot.left) && isSameTree(root.right, subRoot.right);
    }

    public static void main(String[] args) {
        // Creating instances of two trees, s and t
        TreeNode s = new TreeNode(3);
        s.left = new TreeNode(4);
        s.right = new TreeNode(5);
        s.left.left = new TreeNode(1);
        s.left.right = new TreeNode(2);

        TreeNode t = new TreeNode(4);
        t.left = new TreeNode(1);
        t.right = new TreeNode(2);

        // Checking if t is a subtree of s
        boolean isSub = isSubtree(s, t);

        // Output: true
        //          Which means t is a subtree of s
        System.out.println(isSub);
    }
}
```

