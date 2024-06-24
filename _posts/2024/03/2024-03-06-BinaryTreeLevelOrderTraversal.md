---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "102.Binary Tree Level Order Traversal"
date: "2024-03-06"
categories:
  - "LeetCode Trees"
---

# LeetCode 102. Binary Tree Level Order Traversal 

- Given the `root` of a binary tree, return *the level order traversal of its nodes' values*. (i.e., from left to right, level by level).

**Example 1**

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```

**Example 2**

```
Input: root = [1]
Output: [[1]]
```

**Example 3**

```
Input: root = []
Output: []
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Trees;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

/**
 * @author zhengstars
 * @date 2024/04/08
 */
public class BinaryTreeLevelOrderTraversal {
    public static List<List<Integer>> levelOrder(TreeNode root) {
        // Initialize our result list
        List<List<Integer>> res = new ArrayList<>();

        // null-check: if the binary tree is empty, directly return the empty result list.
        if(root == null){
            return res;
        }

        // Initialize our queue with the root node
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        // While our queue is not empty
        while (!queue.isEmpty()){
            // Initialize a new list to store the current level
            List<Integer> level = new ArrayList<>();

            // Get the number of nodes at the current level
            int size = queue.size();

            // Loop over all the nodes at the current level
            for(int i = 0; i < size; i++){
                // Dequeue a node from the front of the queue
                TreeNode node = queue.remove();

                // Add the node's value to the current level's list
                level.add(node.val);

                // Enqueue the current node's left and right children to the queue
                if(node.left != null){
                    queue.add(node.left);
                }

                if(node.right != null){
                    queue.add(node.right);
                }
            }

            // After we've processed all the nodes at the current level, 
            // add the current level's list to our result list.
            res.add(level);
        }

        // Finally, return the result list.
        return res;
    }


    public static void main(String[] args) {

        // Test case 1: a complete binary tree
        TreeNode root = new TreeNode(3);
        root.left = new TreeNode(9);
        root.left.left = new TreeNode(6);
        root.left.right = new TreeNode(2);
        root.right = new TreeNode(20);
        root.right.left = new TreeNode(15);
        root.right.right = new TreeNode(7);
        // Expected output: [[3],[9,20],[6,2,15,7]].
        System.out.println(levelOrder(root));

        // Test case 2: a binary tree with only one node.
        root = new TreeNode(1);
        // Expected output: [[1]].
        System.out.println(levelOrder(root));

        // Test case 3: an empty binary tree (null root).
        root = null;
        // Expected output: [].
        System.out.println(levelOrder(root));
    }
} 
```

