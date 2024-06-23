---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Max Path Sum In Binary Tree"
date: "2023-03-11"
categories:
  - "Algorithm Binary Trees"
---

# Max Path Sum In Binary Tree [Hard]

- Write a function that takes in a Binary Tree and returns its max path sum

- A path is a collection of connected nodes in a tree, where no node is connected to more than two other nodes; a path sum is the sum of the values of the nodes in a particular path.

- Each `BinaryTree` node has an integer `value`, a `left` child node, and a `right` child node. Children nodes can either be `BinaryTree` nodes themselves or `None`/`null`.





**Sample Input **

````
tree1 =               1
                   /     \
                 2         3
               /   \     /   \
             4      5   6     7
````

**Sample Output **

```
18 // 5 + 2 + 1 + 3 + 7
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> If you were to imagine each node in a Binary Tree as the root of the Binary Tree,
 temporarily eliminating all of the nodes that come above it, how would you find the max path sum for each of these newly imagined Binary Trees? In simpler terms, how can you find the max path sum for each subtree in the Binary Tree?</strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> For every node in a Binary Tree, there are four options for the max path sum that includes its value: the node's value alone,the node's value plus the max path sum of its left subtree, the node's value plus the max path sum of its right subtree, or the node's value plus the max path sum of both its subtrees. </strong></i>
</details>



<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> A recursive algorithm that computes each node's max path sum and uses it to compute its parents'nodes'max path sums seems appropriate, but realize that you cannot have a path going through a node and both its subtrees as well as that node's parent node. In other words, the fourth option mentioned in Hint #2 poses a challenge to implementing a recursive algorithm that solves this problem.How can you get around it? </strong></i>
</details>

<br>

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package BT;

/**
 * @author zhengstars
 * @date 2023/02/26
 */
public class MaxPathSumInBinaryTree {
    static class BinaryTree {
        public int value;
        public BinaryTree left;
        public BinaryTree right;

        public BinaryTree(int value) {
            this.value = value;
        }
    }



    public static int maxPathSum(BinaryTree root) {
        // Handle edge case where the tree is empty
        if (null == root) {
            return 0;
        }
        // Initialize the maximum path sum as the smallest possible integer value
        int[] maxSum = {Integer.MIN_VALUE};
        // Recursively find the maximum path sum
        maxPath(root, maxSum);
        // Return the maximum path sum
        return maxSum[0];
    }

    private static int maxPath(BinaryTree root, int[] maxSum) {
        // Base case: the node is null, so the maximum path sum at this node is 0
        if (null == root) {
            return 0;
        }
        // Find the maximum path sum in the left and right subtrees
        int leftMax = Math.max(0, maxPath(root.left, maxSum));
        int rightMax = Math.max(0, maxPath(root.right, maxSum));
        // Calculate the maximum path sum that goes through this node
        int nodeMax = leftMax + rightMax + root.value;
        // Update the maximum path sum seen so far
        maxSum[0] = Math.max(maxSum[0], nodeMax);
        // Return the maximum path sum that goes through either the left or right subtree, plus the value of this node
        return Math.max(leftMax, rightMax) + root.value;
    }

}

```



