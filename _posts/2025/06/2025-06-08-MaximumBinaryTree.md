---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "654. Maximum Binary Tree"
date: "2025-06-08"
tags: Medium
categories:
  - "LeetCode Trees"
---


- You are given an integer array `nums` with no duplicates. A **maximum binary tree** can be built recursively from `nums` using the following algorithm:
  - Create a root node whose value is the maximum value in `nums`.
  - Recursively build the left subtree on the **subarray prefix** to the **left** of the maximum value.
  - Recursively build the right subtree on the **subarray suffix** to the **right** of the maximum value.
- Return *the **maximum binary tree** built from* `nums`.

**Example 1**

```
Input: nums = [3,2,1,6,0,5]
Output: [6,3,5,null,2,0,null,null,1]
Explanation: The recursive calls are as follow:
- The largest value in [3,2,1,6,0,5] is 6. Left prefix is [3,2,1] and right suffix is [0,5].
    - The largest value in [3,2,1] is 3. Left prefix is [] and right suffix is [2,1].
        - Empty array, so no child.
        - The largest value in [2,1] is 2. Left prefix is [] and right suffix is [1].
            - Empty array, so no child.
            - Only one element, so child is a node with value 1.
    - The largest value in [0,5] is 5. Left prefix is [0] and right suffix is [].
        - Only one element, so child is a node with value 0.
        - Empty array, so no child.
```

**Example 2**

```
Input: nums = [3,2,1]
Output: [3,null,2,null,1]
```

## Method 1

```tex
【O(nlog(n)) time | O(n) space】
```

```java
package Leetcode.Trees;

import java.util.*;

/**
 * @Author zhengxingxing
 * @Date 2025/06/08
 */
public class MaximumBinaryTree {

    public static TreeNode constructMaximumBinaryTree(int[] nums) {
        // Call the recursive helper function with the entire array range
        // left boundary = 0 (first index)
        // right boundary = nums.length-1 (last index)
        return construct(nums, 0, nums.length - 1);
    }


    public static TreeNode construct(int[] nums, int left, int right) {
        // BASE CASE: Check if the current range is invalid
        // This happens when we have an empty subarray
        // For example: left=3, right=2 means no elements to process
        if (left > right) {
            return null; // Return null for empty subtree
        }

        // STEP 1: Find the index of the maximum element in current range
        // Initialize 'best' to the left boundary as our starting candidate
        int best = left;

        // LINEAR SEARCH: Iterate through the current range [left, right]
        // to find the index of the maximum element
        for (int i = left; i <= right; i++) {
            // Compare current element with the current maximum candidate
            // If current element is greater, update the best index
            if (nums[i] > nums[best]) {
                best = i; // Update best to current index
            }
        }
        // After this loop, 'best' contains the index of maximum element
        // in the range [left, right]

        // STEP 2: Create the root node using the maximum value
        // nums[best] is guaranteed to be the maximum in current range
        TreeNode node = new TreeNode(nums[best]);

        // STEP 3: DIVIDE AND CONQUER - Recursively build subtrees

        // Build LEFT SUBTREE:
        // Process all elements to the LEFT of the maximum element
        // Range: [left, best-1] (all indices before the maximum)
        // If best == left, then [left, best-1] = [left, left-1] which is empty
        node.left = construct(nums, left, best - 1);

        // Build RIGHT SUBTREE:
        // Process all elements to the RIGHT of the maximum element
        // Range: [best+1, right] (all indices after the maximum)
        // If best == right, then [best+1, right] = [right+1, right] which is empty
        node.right = construct(nums, best + 1, right);

        // STEP 4: Return the constructed subtree
        // The node now has its left and right subtrees properly attached
        return node;
    }

    public static List<Integer> levelOrder(TreeNode root) {
        // Initialize result list to store the traversal
        List<Integer> result = new ArrayList<>();

        // Handle edge case: empty tree
        if (root == null) {
            return result; // Return empty list
        }

        // Use a queue for BFS traversal
        // LinkedList implements Queue interface efficiently
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root); // Add root to start traversal

        // BFS TRAVERSAL: Process nodes level by level
        while (!queue.isEmpty()) {
            // Remove and get the front node from queue
            TreeNode node = queue.poll();

            if (node != null) {
                // Add the node's value to result
                result.add(node.val);

                // Add children to queue for next level processing
                // Note: We add both left and right children regardless of null
                // This helps maintain the tree structure in output
                queue.offer(node.left);
                queue.offer(node.right);
            } else {
                // If current node is null, add null to result
                // This represents missing nodes in the tree structure
                result.add(null);
            }
        }

        // CLEANUP: Remove trailing null values from the result
        // These trailing nulls don't add meaningful information
        while (!result.isEmpty() && result.get(result.size() - 1) == null) {
            result.remove(result.size() - 1);
        }

        return result;
    }

    public static void main(String[] args) {
        // TEST CASE 1: Standard example from the problem
        // Expected tree structure:
        //        6
        //       / \
        //      3   5
        //       \ /
        //        2 0
        //         \
        //          1
        int[] nums1 = {3, 2, 1, 6, 0, 5};
        TreeNode root1 = constructMaximumBinaryTree(nums1);
        System.out.println("Test Case 1:");
        System.out.println("Input: " + Arrays.toString(nums1));
        System.out.println("Output: " + levelOrder(root1));
        System.out.println("Expected: [6, 3, 5, null, 2, 0, null, null, 1]");
        System.out.println();

        // TEST CASE 2: Monotonic decreasing sequence
        // Expected tree structure:
        //      3
        //       \
        //        2
        //         \
        //          1
        int[] nums2 = {3, 2, 1};
        TreeNode root2 = constructMaximumBinaryTree(nums2);
        System.out.println("Test Case 2:");
        System.out.println("Input: " + Arrays.toString(nums2));
        System.out.println("Output: " + levelOrder(root2));
        System.out.println("Expected: [3, null, 2, null, 1]");
        System.out.println();

        // TEST CASE 3: Single element (edge case)
        // Expected tree structure: just one node with value 5
        int[] nums3 = {5};
        TreeNode root3 = constructMaximumBinaryTree(nums3);
        System.out.println("Test Case 3:");
        System.out.println("Input: " + Arrays.toString(nums3));
        System.out.println("Output: " + levelOrder(root3));
        System.out.println("Expected: [5]");
        System.out.println();

        // TEST CASE 4: Monotonic increasing sequence (worst case for time complexity)
        // Expected tree structure:
        //          5
        //         /
        //        4
        //       /
        //      3
        //     /
        //    2
        //   /
        //  1
        int[] nums4 = {1, 2, 3, 4, 5};
        TreeNode root4 = constructMaximumBinaryTree(nums4);
        System.out.println("Test Case 4:");
        System.out.println("Input: " + Arrays.toString(nums4));
        System.out.println("Output: " + levelOrder(root4));
        System.out.println("Expected: [5, 4, null, 3, null, 2, null, 1]");
    }
}
```





