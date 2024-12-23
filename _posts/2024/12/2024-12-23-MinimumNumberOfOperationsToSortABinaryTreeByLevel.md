---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "2471. Minimum Number of Operations to Sort a Binary Tree by Level"
date: "2024-12-23"
tags: Medium
categories:
  - "LeetCode BFS"
---

- You are given the `root` of a binary tree with **unique values**.
- In one operation, you can choose any two nodes **at the same level** and swap their values.
- Return *the minimum number of operations needed to make the values at each level sorted in a **strictly increasing order***.
- The **level** of a node is the number of edges along the path between it and the root node*.*

**Example 1**

```
Input: root = [1,4,3,7,6,8,5,null,null,null,null,9,null,10]
Output: 3
Explanation:
- Swap 4 and 3. The 2nd level becomes [3,4].
- Swap 7 and 5. The 3rd level becomes [5,6,8,7].
- Swap 8 and 7. The 3rd level becomes [5,6,7,8].
We used 3 operations so return 3.
It can be proven that 3 is the minimum number of operations needed.
```

**Example 2**

```
Input: root = [1,3,2,7,6,5,4]
Output: 3
Explanation:
- Swap 3 and 2. The 2nd level becomes [2,3].
- Swap 7 and 4. The 3rd level becomes [4,6,5,7].
- Swap 6 and 5. The 3rd level becomes [4,5,6,7].
We used 3 operations so return 3.
It can be proven that 3 is the minimum number of operations needed.
```

**Example 3**

```
Input: root = [1,2,3,4,5,6]
Output: 0
Explanation: Each level is already sorted in increasing order so return 0.
```

## Method 1

```tex
【O(n*log(n)) time | O(n) space】
```

```java
package Leetcode.BFS;

import java.util.*;

/**
 * @author zhengxingxing
 * @date 2024/12/23
 */
public class MinimumNumberOfOperationsToSortABinaryTreeByLevel {
    public static int minimumOperations(TreeNode root) {
        // Create a queue for level-order traversal
        Queue<TreeNode> queue = new ArrayDeque<>();
        // Track total number of swaps needed
        int ans = 0;
        queue.offer(root);

        while (!queue.isEmpty()) {
            // Get the number of nodes at current level
            int size = queue.size();
            // arr stores original order, temp stores sorted order
            int[] arr = new int[size], temp = new int[size];

            // Process all nodes at current level
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();
                // Store node values in both arrays
                arr[i] = temp[i] = cur.val;
                // Add left and right children to queue for next level
                if (cur.left != null) queue.offer(cur.left);
                if (cur.right != null) queue.offer(cur.right);
            }

            // Create hashmap to store value-to-position mapping after sorting
            Map<Integer, Integer> map = new HashMap<>();
            // Sort temp array to get target order
            Arrays.sort(temp);
            // Map each value to its position in sorted array
            for (int i = 0; i < arr.length; i++) map.put(temp[i], i);

            // Start swapping process
            for (int i = 0; i < arr.length; i++) {
                // Swap until current position has correct value
                while (arr[i] != temp[i]) {
                    // Find position where current value should be
                    int j = map.get(arr[i]);
                    // Swap values
                    int t = arr[i];
                    arr[i] = arr[j];
                    arr[j] = t;
                    // Increment swap counter
                    ans++;
                }
            }
        }
        // Return minimum number of swaps needed
        return ans;
    }

    public static void main(String[] args) {

        // Test Case 1: [1,4,3,7,6,8,5,null,null,null,null,9,null,10]
        TreeNode root1 = new TreeNode(1);
        root1.left = new TreeNode(4);
        root1.right = new TreeNode(3);
        root1.left.left = new TreeNode(7);
        root1.left.right = new TreeNode(6);
        root1.right.left = new TreeNode(8);
        root1.right.right = new TreeNode(5);
        root1.right.left.left = new TreeNode(9);
        root1.right.right.left = new TreeNode(10);
        System.out.println("Test Case 1 Result: " + minimumOperations(root1));  // Expected: 3

        // Test Case 2: [1,3,2,7,6,5,4]
        TreeNode root2 = new TreeNode(1);
        root2.left = new TreeNode(3);
        root2.right = new TreeNode(2);
        root2.left.left = new TreeNode(7);
        root2.left.right = new TreeNode(6);
        root2.right.left = new TreeNode(5);
        root2.right.right = new TreeNode(4);
        System.out.println("Test Case 2 Result: " + minimumOperations(root2));  // Expected: 3

        // Test Case 3: [1,2,3,4,5,6]
        TreeNode root3 = new TreeNode(1);
        root3.left = new TreeNode(2);
        root3.right = new TreeNode(3);
        root3.left.left = new TreeNode(4);
        root3.left.right = new TreeNode(5);
        root3.right.left = new TreeNode(6);
        System.out.println("Test Case 3 Result: " + minimumOperations(root3));  // Expected: 0

        // Test Case 4: Empty tree
        System.out.println("Test Case 4 Result: " + minimumOperations(null));  // Expected: 0

        // Test Case 5: Single node
        TreeNode root5 = new TreeNode(1);
        System.out.println("Test Case 5 Result: " + minimumOperations(root5));  // Expected: 0
    }

    public static void printTree(TreeNode root) {
        if (root == null) {
            System.out.println("Empty tree");
            return;
        }

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                System.out.print(node.val + " ");

                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            System.out.println();  // New line for each level
        }
    }
}

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
```





