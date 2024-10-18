---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "236. Lowest Common Ancestor of a Binary Tree"
date: "2024-10-19"
categories:
  - "LeetCode Trees"
---


- Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.
- According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”


**Example 1**

```
Input: root = [ 3,5,1,6,2,0,8,null,null,7,4 ], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2**

```
Input: root = [ 3,5,1,6,2,0,8,null,null,7,4 ], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

**Example 3**

```
Input: root = [1,2], p = 1, q = 2
Output: 1
```

## Method 1

```tex
【O(n) time | O(h) space】
```

```java
package Leetcode.Trees;

/**
 * @author zhengstars
 * @date 2024/10/19
 */
public class LowestCommonAncestorOfABinaryTree {
    public static TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Base case: If root is null or root matches either p or q, return root
        if (root == null || p == root || q == root) {
            return root;
        }

        // Recursively search for LCA in the left subtree
        TreeNode left = lowestCommonAncestor(root.left, p, q);

        // Recursively search for LCA in the right subtree
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // If both left and right are not null, it means p and q are found in
        // different subtrees of the current root. Thus, the current root is the LCA.
        if (left != null && right != null) {
            return root; // Current root is the LCA
        }

        // If one of the subtrees returned a non-null value, it means both p and q
        // are located in that subtree. If both are null, return null.
        // This will effectively return the found node (either left or right) or null.
        return left != null ? left : right;
    }

    public static void main(String[] args) {
        // Build the binary tree
        TreeNode root = new TreeNode(3);
        TreeNode node5 = new TreeNode(5);
        TreeNode node1 = new TreeNode(1);
        TreeNode node6 = new TreeNode(6);
        TreeNode node2 = new TreeNode(2);
        TreeNode node0 = new TreeNode(0);
        TreeNode node8 = new TreeNode(8);
        TreeNode node7 = new TreeNode(7);
        TreeNode node4 = new TreeNode(4);

        // Construct the tree by linking nodes
        root.left = node5;
        root.right = node1;
        node5.left = node6;
        node5.right = node2;
        node1.left = node0;
        node1.right = node8;
        node2.left = node7;
        node2.right = node4;

        // Test case 1: LCA of 5 and 0
        TreeNode lca1 = lowestCommonAncestor(root, node5, node0);
        System.out.println("LCA of 5 and 0: " + (lca1 != null ? lca1.val : "null")); // Expected output: 3

        // Test case 2: LCA of 5 and 4
        TreeNode lca2 = lowestCommonAncestor(root, node5, node4);
        System.out.println("LCA of 5 and 4: " + (lca2 != null ? lca2.val : "null")); // Expected output: 5

        // Test case 3: LCA of 6 and 4
        TreeNode lca3 = lowestCommonAncestor(root, node6, node4);
        System.out.println("LCA of 6 and 4: " + (lca3 != null ? lca3.val : "null")); // Expected output: 5

        // Test case 4: LCA of 7 and 4
        TreeNode lca4 = lowestCommonAncestor(root, node7, node4);
        System.out.println("LCA of 7 and 4: " + (lca4 != null ? lca4.val : "null")); // Expected output: 2

        // Test case 5: LCA of 2 and 4
        TreeNode lca5 = lowestCommonAncestor(root, node2, node4);
        System.out.println("LCA of 2 and 4: " + (lca5 != null ? lca5.val : "null")); // Expected output: 2
    }
}

```





