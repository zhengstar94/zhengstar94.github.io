---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "124.Binary Tree Maximum Path Sum"
date: "2024-03-09"
categories:
  - "LeetCode Trees"
---

# LeetCode 124. Binary Tree Maximum Path Sum

- A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.
- The **path sum** of a path is the sum of the node's values in the path.
- Given the `root` of a binary tree, return *the maximum **path sum** of any **non-empty** path*.


**Example 1**

```
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
```

**Example 2**

```
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Trees;

/**
 * @author zhengstars
 * @date 2024/04/19
 */
public class BinaryTreeMaximumPathSum {
    // A variable to keep the maximum path sum
    private static int maxSum = Integer.MIN_VALUE;

    // The main function to find the maximum path sum.
    public static int maxPathSum(TreeNode root) {
        maxGain(root);
        return maxSum;
    }

    // A helper function to find the maximum gain of each node.
    private static int maxGain(TreeNode node) {
        // Base case: if the node is null, return 0
        if (node == null) {
            return 0;
        }

        // Recursive case
        // Calculate the maximum sum of the left and right subtrees. If a subtree sum is negative, we ignore it and return 0.
        int leftGain = Math.max(maxGain(node.left), 0);
        int rightGain = Math.max(maxGain(node.right), 0);

        // Check if the price of the new path would be greater than the current maxSum
        int priceNewPath = node.val + leftGain + rightGain;

        // Update the maxSum if the price of the new path is greater
        maxSum = Math.max(maxSum, priceNewPath);

        // Return the max gain if we would continue the same path
        return node.val + Math.max(leftGain, rightGain);
    }

    public static void main(String[] args) {
        // Test cases
        TreeNode test1 = new TreeNode(1, new TreeNode(2), new TreeNode(3));
        System.out.println(maxPathSum(test1));  // output: 6

        TreeNode test2 = new TreeNode(-10, new TreeNode(9),
                new TreeNode(20, new TreeNode(15), new TreeNode(7)));
        System.out.println(maxPathSum(test2));  // Output: 42
    }
}
```

