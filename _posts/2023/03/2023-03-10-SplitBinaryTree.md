---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Split Binary Tree"
date: "2023-03-10"
categories:
  - "Algorithm Binary Trees"
---

# Split Binary Tree [Medium]

- Write a function that takes in a Binary Tree with at least one node and checks if that Binary
  Tree can be split into two Binary Trees of equal sum by removing a single edge. If this split is possible, return the new sum of each Binary Tree, otherwise return 0. Note that you do not need to return the edge that was removed.
- The sum of a Binary Tree is the sum of all values in that Binary Tree.
- Each `BinaryTree` node has an integer `value`, a `left` child node, and a `right` child node. Children nodes can either be `BinaryTree` nodes themselves or `None`/ `null.`

**Sample Input**

```
tree =       1
					/     \
			  3	       -2
			/  \      /   \
		 6	  -5   5     2
	 /   
	2   
```

**Sample Output**

```
  // Remove the edge to the left of the root node,
6 // creating two trees,each with sums of 6
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Try first calculating the sum of the entire Binary Tree. What information does this give you towards solving the problem?</strong></i>
</details>



<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> If the sum of the entire Binary Tree is odd, then there is no possible solution, because the values are all integers.Otherwise, the solution could be that sum divided by two, or potentially there is still no solution. What does the scenario look like where the solution is the sum divided by two? </strong></i>
</details>

<br>

<details> <summary><b>Hint 3</b></summary>
    <br>
    <i><strong> There is a solution if there is a subtree that has a sum equal to thethe total Binary Tree sum divided by two. In this case, removing the incoming edge to that node would have to create another Binary Tree of equal sum. </strong></i>
</details>

<br>

<details> <summary><b>Hint 4</b></summary>
    <br>
    <i><strong> To prevent recalculating the same subtree sums,try using a post-order traversal of the Binary Tree. This allows you to calculate the sums of the smallest subtrees first, then send that information back up to the parents to quickly calculate their sums. </strong></i>
</details>

<br>

## Method 1

```tex
【O(n)time∣O(h)space】
```

```java
package BT;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2023/05/01
 */
public class SplitBinaryTree {
    static class BinaryTree {
        public int value;
        public BinaryTree left = null;
        public BinaryTree right = null;

        public BinaryTree(int value) {
            this.value = value;
        }
    }

    /**
     * Returns the value of a node in the input binary tree which, if removed, will cause the tree
     * to be split into two trees with equal sum.
     *
     * @param tree The binary tree to check.
     * @return The value of a node in the tree which, if removed, will cause the tree to be split into
     *         two trees with equal sum. Returns 0 if no such node exists.
     */
    public int splitBinaryTree(BinaryTree tree) {
        List<Integer> sums = new ArrayList<>();
        int total = helper(sums, tree);
        if (total % 2 != 0) {
            // If the total sum of the tree is odd, the tree cannot be split into two trees with equal sum.
            return 0;
        }

        // Iterate through the sums of all subtrees of the input tree and return the value of the first
        // node found which, if removed, would cause the tree to be split into two trees with equal sum.
        for (int sum : sums) {
            if (sum == total / 2) {
                return sum;
            }
        }
        // If no such node exists, return 0.
        return 0;
    }

    /**
     * Calculates the sum of the input binary tree and all of its subtrees.
     *
     * @param sums A list to which the sum of each subtree will be added.
     * @param tree The binary tree to calculate the sum of.
     * @return The sum of the input binary tree and all of its subtrees.
     */
    private int helper(List<Integer> sums, BinaryTree tree) {
        if (tree == null) {
            // If the current node is null, return 0.
            return 0;
        }
        int left = helper(sums, tree.left);
        int right = helper(sums, tree.right);
        // Calculate the sum of the current subtree and add it to the list of sums.
        int sum = tree.value + left + right;
        sums.add(sum);
        return sum;
    }
}

```

