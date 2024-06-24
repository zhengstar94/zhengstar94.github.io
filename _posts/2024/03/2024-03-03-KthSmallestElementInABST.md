---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "230.Kth Smallest Element in a BST"
date: "2024-03-03"
categories:
  - "LeetCode Trees"
---

# LeetCode 230. Kth Smallest Element in a BST

- Given the `root` of a binary search tree, and an integer `k`, return *the* `kth` *smallest value (**1-indexed**) of all the values of the nodes in the tree*.


**Example 1**

```
Input: root = [3,1,4,null,2], k = 1
Output: 1
```

**Example 2**

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Trees;

/**
 * @author zhengstars
 * @date 2024/04/11
 */

public class KthSmallestElementinaBST {

    // This is a counter to keep count of the nodes visited in the BST
    private static int count = 0;
    // This variable stores the result i.e., the kth smallest element of the BST
    private static int result = 0;

    public static int kthSmallest(TreeNode root, int k) {
        // This function is called which traverses the BST in inorder fashion
        traverse(root, k);
        // The result is returned
        return result;
    }
    
    private static void traverse(TreeNode root, int k) {
        // If the root is null, no operation is performed
        if (root == null) {
            return;
        }

        // Traversing the left subtree first
        traverse(root.left, k);

        // Current node is visited and the counter is incremented by 1
        count++;
        // If the counter equals k, this means that the current node is the required kth smallest node. Hence, its value is stored in result
        if (count == k) {
            result = root.val;
        }

        // If the counter is less than k, the right subtree of the current node is traversed
        if (count < k) {
            traverse(root.right, k);
        }
    }

    // This is a testing function
    public static void main(String[] args) {
        // Creating a BST
        TreeNode root = new TreeNode(5);
        root.left = new TreeNode(3);
        root.right = new TreeNode(6);
        root.left.left = new TreeNode(2);
        root.left.right = new TreeNode(4);
        root.left.left.left = new TreeNode(1);

        // Expected output: 3
        System.out.println(kthSmallest(root, 3));
    }
}
```

