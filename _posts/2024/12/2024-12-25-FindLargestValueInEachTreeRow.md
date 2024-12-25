---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "515. Find Largest Value in Each Tree Row"
date: "2024-12-25"
tags: Medium
categories:
  - "LeetCode BFS"
---


- Given the `root` of a binary tree, return *an array of the largest value in each row* of the tree **(0-indexed)**.

**Example 1**

```
Input: root = [1,3,2,5,3,null,9]
Output: [1,3,9]
```

**Example 2**

```
Input: root = [1,2,3]
Output: [1,3]
```

## Method 1

```tex
【O(n) time | O(w) space】
```

```java
package Leetcode.BFS;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

/**
 * @author zhengxingxing
 * @date 2024/12/25
 */
public class FindLargestValueInEachTreeRow {
    public static List<Integer> largestValues(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }

        // Queue for BFS traversal
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            // Get the size of current level
            int levelSize = queue.size();
            int maxVal = Integer.MIN_VALUE;

            // Process all nodes in current level
            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll();
                maxVal = Math.max(maxVal, node.val);

                // Add children to queue for next level processing
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }

            // Add maximum value of current level to result
            result.add(maxVal);
        }

        return result;
    }

    public static void main(String[] args) {

        // Test Case 1: Regular binary tree
        TreeNode root1 = new TreeNode(1);
        root1.left = new TreeNode(3);
        root1.right = new TreeNode(2);
        root1.left.left = new TreeNode(5);
        root1.left.right = new TreeNode(3);
        root1.right.right = new TreeNode(9);
        System.out.println("Test Case 1 - Expected: [1, 3, 9], Output: " +
                largestValues(root1));

        // Test Case 2: Single node tree
        TreeNode root2 = new TreeNode(1);
        System.out.println("Test Case 2 - Expected: [1], Output: " +
                largestValues(root2));

        // Test Case 3: Empty tree
        TreeNode root3 = null;
        System.out.println("Test Case 3 - Expected: [], Output: " +
                largestValues(root3));

        // Test Case 4: Complete binary tree
        TreeNode root4 = new TreeNode(1);
        root4.left = new TreeNode(2);
        root4.right = new TreeNode(3);
        root4.left.left = new TreeNode(4);
        root4.left.right = new TreeNode(5);
        root4.right.left = new TreeNode(6);
        root4.right.right = new TreeNode(7);
        System.out.println("Test Case 4 - Expected: [1, 3, 7], Output: " +
                largestValues(root4));

        // Test Case 5: Unbalanced tree
        TreeNode root5 = new TreeNode(1);
        root5.left = new TreeNode(2);
        root5.left.left = new TreeNode(4);
        root5.left.left.left = new TreeNode(8);
        System.out.println("Test Case 5 - Expected: [1, 2, 4, 8], Output: " +
                largestValues(root5));
    }
}

```





