---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1367. Linked List in Binary Tree"
date: "2024-12-30"
tags: Medium
categories:
  - "LeetCode Trees"
---

- Given a binary tree `root` and a linked list with `head` as the first node.
- Return True if all the elements in the linked list starting from the `head` correspond to some *downward path* connected in the binary tree otherwise return False.
- In this context downward path means a path that starts at some node and goes downwards.

**Example 1**

```
Input: head = [4,2,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
Output: true
Explanation: Nodes in blue form a subpath in the binary Tree.  
```

**Example 2**

```
Input: head = [1,4,2,6], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
Output: true
```

**Example 3**

```
Input: head = [1,4,2,6,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
Output: false
Explanation: There is no path in the binary tree that contains all the elements of the linked list from head.
```

## Method 1

```tex
【O(N × min(L, H)) time | O(H) space】
```

```java
package Leetcode.Trees;

/**
* @author zhengxingxing
* @date 2024/12/30
 */
public class LinkedListInBinaryTree {
    public static boolean isSubPath(ListNode head, TreeNode root) {
        // Empty list is always a valid path
        if(head == null){
            return true;
        }

        // Empty tree can't contain any path
        if(root == null){
            return false;
        }

        // First, try to match the entire linked list starting from current tree node
        if (dfs(head, root)) {
            return true;
        }

        /**
         * If current node doesn't lead to a match, try finding a match in left or right subtrees.
         * This is different from the DFS below because:
         * 1. It looks for a completely new starting point for the entire linked list
         * 2. It resets back to the head of the linked list for each new tree node
         * 3. It explores every possible starting point in the tree
         * Example: If we're looking for [4,2,8] and current node is 1, we check if either
         * the left or right subtree contains the entire sequence [4,2,8] starting from any node
         */
        return isSubPath(head, root.left) || isSubPath(head, root.right);
    }

    private static boolean dfs(ListNode head, TreeNode root) {
        // Matched entire linked list
        if(head == null){
            return true;
        }

        // Reached end of tree path without full match
        if(root == null){
            return false;
        }

        // Current nodes must match to continue
        if (head.val != root.val) {
            return false;
        }

        /**
         * Continue matching the rest of the linked list with either left or right child.
         * This is different from the isSubPath recursion above because:
         * 1. It continues an existing match (uses head.next instead of head)
         * 2. Only looks for the next value in the current path
         * 3. Doesn't try to restart the sequence from the beginning
         * Example: If we matched first node 4, we only look for the next value 2
         * in the immediate children of current node
         */
        return dfs(head.next, root.left) || dfs(head.next, root.right);
    }

    public static void main(String[] args) {

        // Test Case 1: Example from the problem statement
        TreeNode root1 = new TreeNode(1);
        root1.left = new TreeNode(4);
        root1.right = new TreeNode(4);
        root1.left.right = new TreeNode(2);
        root1.left.right.left = new TreeNode(1);
        root1.right.left = new TreeNode(2);
        root1.right.left.left = new TreeNode(6);
        root1.right.left.right = new TreeNode(8);
        root1.right.left.right.left = new TreeNode(1);
        root1.right.left.right.right = new TreeNode(3);

        ListNode head1 = createLinkedList(new int[]{4, 2, 8});
        System.out.println("Test Case 1 Expected: true");
        System.out.println("Test Case 1 Result: " + isSubPath(head1, root1));

        // Test Case 2: Simple tree with matching path
        TreeNode root2 = new TreeNode(1);
        root2.left = new TreeNode(2);
        root2.right = new TreeNode(3);
        root2.left.left = new TreeNode(4);

        ListNode head2 = createLinkedList(new int[]{2, 4});
        System.out.println("Test Case 2 Expected: true");
        System.out.println("Test Case 2 Result: " + isSubPath(head2, root2));

        // Test Case 3: No matching path
        TreeNode root3 = new TreeNode(1);
        root3.left = new TreeNode(2);
        root3.right = new TreeNode(3);

        ListNode head3 = createLinkedList(new int[]{4, 5});
        System.out.println("Test Case 3 Expected: false");
        System.out.println("Test Case 3 Result: " + isSubPath(head3, root3));
    }

    private static ListNode createLinkedList(int[] values) {
        if (values == null || values.length == 0) {
            return null;
        }
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        for (int val : values) {
            current.next = new ListNode(val);
            current = current.next;
        }
        return dummy.next;
    }
}

class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}
```





