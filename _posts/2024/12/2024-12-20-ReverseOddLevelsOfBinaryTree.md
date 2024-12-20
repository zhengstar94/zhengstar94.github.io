---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2415. Reverse Odd Levels of Binary Tree"
date: "2024-12-20"
tags: Medium
categories:
  - "LeetCode Dynamic Programming"
---

- Given the `root` of a **perfect** binary tree, reverse the node values at each **odd** level of the tree.
  - For example, suppose the node values at level 3 are `[2,1,3,4,7,11,29,18]`, then it should become `[18,29,11,7,4,3,1,2]`.
- Return *the root of the reversed tree*.
- A binary tree is **perfect** if all parent nodes have two children and all leaves are on the same level.
- The **level** of a node is the number of edges along the path between it and the root node.


**Example 1**

```
Input: root = [2,3,5,8,13,21,34]
Output: [2,5,3,8,13,21,34]
Explanation: 
The tree has only one odd level.
The nodes at level 1 are 3, 5 respectively, which are reversed and become 5, 3.
```

**Example 2**

```
Input: root = [7,13,11]
Output: [7,11,13]
Explanation: 
The nodes at level 1 are 13, 11, which are reversed and become 11, 13.
```

**Example 3**

```
Input: root = [0,1,2,0,0,0,0,1,1,1,1,2,2,2,2]
Output: [0,2,1,0,0,0,0,2,2,2,2,1,1,1,1]
Explanation: 
The odd levels have non-zero values.
The nodes at level 1 were 1, 2, and are 2, 1 after the reversal.
The nodes at level 3 were 1, 1, 1, 1, 2, 2, 2, 2, and are 2, 2, 2, 2, 1, 1, 1, 1 after the reversal.
```

## Method 1

```tex
【O(n) time | O(h) space】
```

```java
package Leetcode.DynamicProgramming;

import java.util.LinkedList;
import java.util.Queue;

/**
* @author zhengxingxing
* @date 2024/12/20
 **/

class TreeNode {
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


public class ReverseOddLevelsOfBinaryTree {
    public static TreeNode reverseOddLevels(TreeNode root) {
        // Start processing from level 1 (children of root)
        dfs(root.left, root.right, true);
        return root;
    }

    private static void dfs(TreeNode left, TreeNode right, boolean isOdd) {
        // Return if either node is null (reached leaf)
        if(left == null || right == null){
            return;
        }

        // If at odd level, swap the values
        if(isOdd) {
            int tmp = left.val;
            left.val = right.val;
            right.val = tmp;
        }

        // Recursively process next level
        // Process symmetric pairs: left's left child with right's right child
        dfs(left.left, right.right, !isOdd);
        // Process symmetric pairs: left's right child with right's left child
        dfs(left.right, right.left, !isOdd);
    }

    public static void main(String[] args) {

        // Test Case 1: Simple tree with 3 levels
        TreeNode root1 = new TreeNode(1);
        root1.left = new TreeNode(2);
        root1.right = new TreeNode(3);
        root1.left.left = new TreeNode(4);
        root1.left.right = new TreeNode(5);
        root1.right.left = new TreeNode(6);
        root1.right.right = new TreeNode(7);

        System.out.println("Test Case 1 - Before:");
        printLevelOrder(root1);
        reverseOddLevels(root1);
        System.out.println("Test Case 1 - After:");
        printLevelOrder(root1);

        // Test Case 2: Tree with 4 levels
        TreeNode root2 = new TreeNode(1);
        root2.left = new TreeNode(2);
        root2.right = new TreeNode(3);
        root2.left.left = new TreeNode(4);
        root2.left.right = new TreeNode(5);
        root2.right.left = new TreeNode(6);
        root2.right.right = new TreeNode(7);
        root2.left.left.left = new TreeNode(8);
        root2.left.left.right = new TreeNode(9);
        root2.left.right.left = new TreeNode(10);
        root2.left.right.right = new TreeNode(11);
        root2.right.left.left = new TreeNode(12);
        root2.right.left.right = new TreeNode(13);
        root2.right.right.left = new TreeNode(14);
        root2.right.right.right = new TreeNode(15);

        System.out.println("\nTest Case 2 - Before:");
        printLevelOrder(root2);
        reverseOddLevels(root2);
        System.out.println("Test Case 2 - After:");
        printLevelOrder(root2);
    }

    // Helper method to print tree in level order
    private static void printLevelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll();
                System.out.print(node.val + " ");
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
            System.out.println();
        }
    }
}

```





