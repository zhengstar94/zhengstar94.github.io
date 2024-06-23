---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Evaluate Expression Tree"
date: "2023-03-03"
categories:
  - "Algorithm Binary Trees"
---

# Evaluate Expression Tree [Easy]

- You're given a binary expression tree. Write a function to evaluate this tree mathematically and return a single resulting integer.
- All leaf nodes in the tree represent operands, which will always be positive integers. All of the other nodes represent operators. There are 4 operators supported, each of which is represented by a negative integer:
  - `1`: Addition operator, adding the left and right subtrees.
  - `2`: Subtraction operator, subtracting the right subtree from the left subtree.
  - `3`: Division operator, dividing the left subtree by the right subtree. If the result is a decimal, it should be rounded towards zero.
  - `4`: Multiplication operator, multiplying the left and right subtrees
- You can assume the tree will always be a valid expression tree. Each operator also works as a grouping symbol, meaning the bottom of the tree is always evaluated first, regardless of the operator.



**Sample Input**

```
tree =     -1
          /   \
       -2     -3
      /  \   /  \
    -4    2  8   3
   /  \
  2   3
```



**Sample Output**

```
6 // (((2 * 3) - 2) + (8 / 3))
```



**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> This problem will be easiest to solve using recursion.Can you think of what the recursive subproblems would be? And what is the base case?</strong></i>
</details>




<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong> For each operator, a recursive call can be made on its left and right values. The result of these recursive calls can then be combined using that operator. The base case to finish recursing will be when we reach an operand,which is any positive integer.</strong></i>
</details>

<br>

## Method 1

```tex
【O(n)time∣O(h)space】
```

```java
package BT;

/**
 * @author zhengstars
 * @date 2023/05/01
 */
public class EvaluateExpressionTree {
    static class BinaryTree {
        public int value;
        public BinaryTree left = null;
        public BinaryTree right = null;

        public BinaryTree(int value) {
            this.value = value;
        }
    }


    public static int evaluateExpressionTree(BinaryTree tree) {
        // if the tree is null, return 0
        if (tree == null) {
            return 0;
        }

        // if the tree is a leaf node, return its value
        if (tree.left == null && tree.right == null) {
            return tree.value;
        }

        // evaluate the left and right subtrees
        int leftValue = evaluateExpressionTree(tree.left);
        int rightValue = evaluateExpressionTree(tree.right);

        // evaluate the current node based on its operator
        switch (tree.value) {
            // addition operator
            case -1:
                return leftValue + rightValue;
            // subtraction operator
            case -2:
                return leftValue - rightValue;
            // division operator
            case -3:
                return leftValue / rightValue;
            // multiplication operator
            case -4:
                return leftValue * rightValue;
            // invalid operator    
            default:
                return 0;
        }
    }
}
```

