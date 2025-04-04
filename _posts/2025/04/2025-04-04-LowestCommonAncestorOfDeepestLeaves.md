---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1123. Lowest Common Ancestor of Deepest Leaves"
date: "2025-04-04"
tags: Medium
categories:
  - "LeetCode Trees"
---


- Given the `root` of a binary tree, return *the lowest common ancestor of its deepest leaves*.
- Recall that:
  - The node of a binary tree is a leaf if and only if it has no children
  - The depth of the root of the tree is `0`. if the depth of a node is `d`, the depth of each of its children is `d + 1`.
  - The lowest common ancestor of a set `S` of nodes, is the node `A` with the largest depth such that every node in `S` is in the subtree with root `A`.

**Example 1**

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4]
Output: [2,7,4]
Explanation: We return the node with value 2, colored in yellow in the diagram.
The nodes coloured in blue are the deepest leaf-nodes of the tree.
Note that nodes 6, 0, and 8 are also leaf nodes, but the depth of them is 2, but the depth of nodes 7 and 4 is 3.
```

**Example 2**

```
Input: root = [1]
Output: [1]
Explanation: The root is the deepest node in the tree, and it's the lca of itself.
```

**Example 3**

```
Input: root = [0,1,3,null,2]
Output: [2]
Explanation: The deepest leaf node in the tree is 2, the lca of one node is itself.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Trees;

/**
 * @author zhengxingxing
 * @date 2025/04/04
 */
public class LowestCommonAncestorOfDeepestLeaves {
    // Store the result node (lowest common ancestor)
    private static TreeNode ans;
    // Track the maximum depth in the entire tree
    private static int maxDepth = -1;


    public static TreeNode lcaDeepestLeaves(TreeNode root) {
        // Reset static variables for multiple test cases
        ans = null;
        maxDepth = -1;
        // Start DFS from root with initial depth 0
        dfs(root, 0);
        return ans;
    }


    private static int dfs(TreeNode node, int depth) {
        // Base case: reached null node
        // Update maxDepth and return current depth
        if (node == null) {
            maxDepth = Math.max(maxDepth, depth);
            return depth;
        }

        // Recursively process left and right subtrees
        int leftMaxDepth = dfs(node.left, depth + 1);
        int rightMaxDepth = dfs(node.right, depth + 1);

        // If both subtrees reach the same maximum depth
        // and this depth equals the tree's maximum depth,
        // current node is a candidate for LCA
        if (leftMaxDepth == rightMaxDepth && leftMaxDepth == maxDepth) {
            ans = node;
        }

        // Return the maximum depth reached in this subtree
        return Math.max(leftMaxDepth, rightMaxDepth);
    }


    public static void main(String[] args) {
        // Test Case 1: Balanced tree
        //       1
        //      / \
        //     2   3
        //    /     \
        //   4       5
        TreeNode test1 = new TreeNode(1);
        test1.left = new TreeNode(2);
        test1.right = new TreeNode(3);
        test1.left.left = new TreeNode(4);
        test1.right.right = new TreeNode(5);

        TreeNode result1 = lcaDeepestLeaves(test1);
        System.out.println("Test Case 1 - Expected: 1, Actual: " + result1.val);

        // Test Case 2: Unbalanced tree
        //       1
        //      / \
        //     2   3
        //    / \
        //   4   5
        //  /   /
        // 6   7
        TreeNode test2 = new TreeNode(1);
        test2.left = new TreeNode(2);
        test2.right = new TreeNode(3);
        test2.left.left = new TreeNode(4);
        test2.left.right = new TreeNode(5);
        test2.left.left.left = new TreeNode(6);
        test2.left.right.left = new TreeNode(7);

        TreeNode result2 = lcaDeepestLeaves(test2);
        System.out.println("Test Case 2 - Expected: 2, Actual: " + result2.val);

        // Test Case 3: Single node tree
        TreeNode test3 = new TreeNode(1);
        TreeNode result3 = lcaDeepestLeaves(test3);
        System.out.println("Test Case 3 - Expected: 1, Actual: " + result3.val);
    }
}


```





