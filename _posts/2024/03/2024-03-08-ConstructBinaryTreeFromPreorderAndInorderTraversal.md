---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "105.Construct Binary Tree from Preorder and Inorder Traversal"
date: "2024-03-08"
categories:
  - "LeetCode Trees"
---

# LeetCode 105. Construct Binary Tree from Preorder and Inorder Traversal

- Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return *the binary tree*.


**Example 1**

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

**Example 2**

```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Trees;

import java.util.LinkedList;
import java.util.Queue;

/**
 * @author zhengstars
 * @date 2024/04/12
 */
public class ConstructBinaryTreeFromPreorderAndInorderTraversal {

    public static TreeNode buildTree(int[] preorder, int[] inorder) {
        return helper(0, 0, inorder.length - 1, preorder, inorder);
    }

    /**
     * A helper function to construct a binary tree.
     * @param preStart The start position in the preorder array from which the subtree will be constructed.
     * @param inStart  The start position in the inorder array from which the subtree will be constructed.
     * @param inEnd  The end position in the inorder array where the subtree construction will stop.
     * @param preorder  The preorder traversal array.
     * @param inorder  The inorder traversal array.
     * @return The root node of the constructed subtree.
     */
    public static TreeNode helper(int preStart, int inStart, int inEnd, int[] preorder, int[] inorder) {
        // If there are no more elements to construct the binary tree, end the recursion.
        if (preStart > preorder.length - 1 || inStart > inEnd) {
            return null;
        }

        // Take the first element of the preorder traversal as the root node.
        TreeNode root = new TreeNode(preorder[preStart]);
        // Locate the root node in the inorder traversal
        int inIndex = inStart;
        for (int i = inStart; i <= inEnd; i++) {
            if (inorder[i] == root.val) {
                inIndex = i;
                break;
            }
        }

        // Construct the left subtree by recursively calling the helper function with updated parameters.
        // The next element in the preorder array is taken as the root of this subtree;
        // elements between the start of the inorder array and the index of the current root minus one form this subtree.
        root.left = helper(preStart + 1, inStart, inIndex - 1, preorder, inorder);

        // The right subtree is constructed in a similar way, using the parts of the preorder and inorder arrays corresponding to the right subtree.
        // The recursion begins from the element in the preorder array immediately following the last element of the previously constructed left subtree;
        // elements located between the index of the current root plus one and the end of the inorder array form this subtree.
        root.right = helper(preStart + inIndex - inStart + 1, inIndex + 1, inEnd, preorder, inorder);

        return root;
    }

    public static void main(String[] args) {

        // Test case1
        int[] preorder1 = {3,9,20,15,7};
        int[] inorder1 = {9,3,15,20,7};
        TreeNode root1 = buildTree(preorder1, inorder1);
        printTree(root1);  // The print result should be：3 9 20 null null 15 7

        // Test case2
        int[] preorder2 = {-1};
        int[] inorder2 = {-1};
        TreeNode root2 = buildTree(preorder2, inorder2);
        printTree(root2);  // The print result should be：-1

        // Test case3
        int[] preorder3 = {1,2,3,4,5};
        int[] inorder3 = {1,2,3,4,5};
        TreeNode root3 = buildTree(preorder3, inorder3);
        printTree(root3);  // The print result should be：1 null 2 null 3 null 4 null 5

        // Test case4
        int[] preorder4 = {7,10,4,3,1,2,8,9};
        int[] inorder4 = {4,10,3,1,7,2,8,9};
        TreeNode root4 = buildTree(preorder4, inorder4);
        printTree(root4);  // The print result should be：7 10 2 4 3 null 8 null null 1 null 9
    }

    // A helper function for level-order traversal (also known as breadth-first traversal) and printing the tree structure.
    public static void printTree(TreeNode root) {
        if (root == null) {
            return;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (node == null) {
                System.out.print("null ");
            } else {
                System.out.print(node.val + " ");
                queue.offer(node.left);
                queue.offer(node.right);
            }
        }

        System.out.println();
    }

}
```

