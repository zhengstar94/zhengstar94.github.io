---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Maximum sum of Non-adjacent nodes"
date: "2023-06-10"
categories:
  - "Algorithm Dynamic"
---

# Maximum sum of Non-adjacent nodes [Medium]

- Given a binary tree with a value associated with each node, we need to choose a subset of these nodes such that sum of chosen nodes is maximum under a constraint that no two chosen node in subset should be directly connected that is, if we have taken a node in our sum then we can’t take its any children or parents in consideration and vice versa.

**Example 1**

```
Input:
     11
    /  \
   1    2
Output: 11
Explanation: The maximum sum is sum of
node 11.
```

**Example 2**

```
Input:
        1
      /   \
     2     3
    /     /  \
   4     5    6
Output: 16
Explanation: The maximum sum is sum of
nodes 1 4 5 6 , i.e 16. These nodes are
non adjacent.
```

<br>

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Dynamic;

/**
 * @author zhengstars
 * @date 2023/12/18
 */
public class MaxSubsetSumNoAdjacent {

    /**
     * Representation of a node in a binary tree
     */
    public static class TreeNode {
        int val;
        TreeNode left, right;

        // Constructor: setting the value of the node and initializing its left and right children as null
        public TreeNode(int val) {
            this.val = val;
            this.left = this.right = null;
        }
    }

    /**
     * Helper function which calls the helper function and return the maximum sum, either inclusive or exclusive of the current node
     * @param root
     * @return
     */
    public static int maxSum(TreeNode root) {
        // result[0] represents max sum excluding current node, result[1] represents max sum including current node
        int[] result = new int[2];
        helper(root, result);
        return Math.max(result[0], result[1]);
    }

    /**
     * Recursive function which calculates both maximum sum inclusive and exclusive for the current node
     * @param node
     * @param result
     * @return
     */
    private static int helper(TreeNode node, int[] result) {
        // base case: if the node is null, then maximum sum is 0
        if (node == null) {
            result[0] = 0;
            result[1] = 0;
            return 0;
        }

        // Arrays to store the maximum sum of left child and right child
        int[] leftResult = new int[2];
        int[] rightResult = new int[2];

        // Recursive call for left child and right child of current node
        helper(node.left, leftResult);
        helper(node.right, rightResult);

        // maximum sum excluding current node is the sum of the maximum sum of its left child and right child
        result[0] = leftResult[1] + rightResult[1];

        // maximum sum including current node is the maximum sum of its left child and right child when they are excluded, plus current node's value
        result[1] = node.val + leftResult[0] + rightResult[0];

        // return the maximum sum for current node, either inclusive or exclusive of the current node
        return Math.max(result[0], result[1]);
    }
    
    public static void main(String[] args) {

        // Example 1
        TreeNode root1 = new TreeNode(11);
        root1.left = new TreeNode(1);
        root1.right = new TreeNode(2);
        int result1 = maxSum(root1);
        System.out.println("Example 1: " + result1);

        // Example 2
        TreeNode root2 = new TreeNode(1);
        root2.left = new TreeNode(2);
        root2.right = new TreeNode(3);
        root2.left.left = new TreeNode(4);
        root2.right.left = new TreeNode(5);
        root2.right.right = new TreeNode(6);
        int result2 = maxSum(root2);
        System.out.println("Example 2: " + result2);
    }


}
```
