---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "199.Binary Tree Right Side View"
date: "2024-11-05"
categories:
  - "LeetCode Trees"
---


- Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return *the values of the nodes you can see ordered from top to bottom*.

**Example 1**

```
Input: root = [1,2,3,null,5,null,4]
Output: [1,3,4]
```

**Example 2**

```
Input: root = [1,null,3]
Output: [1,3]
```

**Example 3**

```
Input: root = []
Output: []
```

## Method 1

```tex
【O(n) time | O(h) space】
```

```java
package Leetcode.Trees;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2024/11/05
 */
public class BinaryTreeRightSideView {
    public static List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        // Start DFS with depth 0 from root
        dfs(root, 0, result);
        return result;
    }

    private static void dfs(TreeNode node,int depth, List<Integer> ans){
        // Base case: if node is null, return
        if(node == null) {
            return;
        }

        /**
         * Key insight: if depth equals ans.size(), this is the first node
         * we're seeing at this depth level. Since we process right before left,
         * this will be the rightmost node at this level.
         */
        if(ans.size() == depth) {
            ans.add(node.val);
        }

        // Process right subtree first to ensure we see rightmost nodes first
        dfs(node.right, depth + 1, ans);

        /**
         * IMPORTANT: Why we need to process the left subtree
         * 1. Handles cases where right subtree is shorter than left
         * 2. Ensures we don't miss nodes that are visible from right when there's no right node
         * 3. Example case:
         *        1
         *       /
         *      2
         *     /
         *    3
         * Here, nodes 2 and 3 should be visible from right even though they're in left subtree
         */
        dfs(node.left, depth + 1, ans);
    }

    public static void main(String[] args) {
        // Test Case 1: Regular tree with right and left nodes
        TreeNode root1 = new TreeNode(1);
        root1.left = new TreeNode(2);
        root1.right = new TreeNode(3);
        root1.left.right = new TreeNode(5);
        root1.right.right = new TreeNode(4);
        System.out.println("Test Case 1: " + rightSideView(root1)); // [1,3,4]

        // Test Case 2: Tree with visible left node
        TreeNode root2 = new TreeNode(1);
        root2.left = new TreeNode(2);
        root2.right = new TreeNode(3);
        root2.left.left = new TreeNode(4);
        System.out.println("Test Case 2: " + rightSideView(root2)); // [1,3,4]

        // Test Case 3: Tree with only left subtree
        TreeNode root3 = new TreeNode(1);
        root3.left = new TreeNode(2);
        root3.left.left = new TreeNode(4);
        root3.left.right = new TreeNode(5);
        System.out.println("Test Case 3: " + rightSideView(root3)); // [1,2,5]

        // Test Case 4: Empty tree
        System.out.println("Test Case 4: " + rightSideView(null)); // []

        // Test Case 5: Deep left-skewed tree
        TreeNode root5 = new TreeNode(1);
        root5.left = new TreeNode(2);
        root5.left.left = new TreeNode(4);
        root5.left.left.left = new TreeNode(5);
        System.out.println("Test Case 5: " + rightSideView(root5)); // [1,2,4,5]
    }
}
```