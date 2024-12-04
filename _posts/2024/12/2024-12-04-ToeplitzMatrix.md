---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "766. Toeplitz Matrix"
date: "2024-12-04"
tags: Easy
categories:
  - "LeetCode Array"
---

- Given an `m x n` `matrix`, return *`true` if the matrix is Toeplitz. Otherwise, return `false`.*
- A matrix is **Toeplitz** if every diagonal from top-left to bottom-right has the same elements.

**Example 1**

```
Input: matrix = [[1,2,3,4],[5,1,2,3],[9,5,1,2]]
Output: true
Explanation:
In the above grid, the diagonals are:
"[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]".
In each diagonal all elements are the same, so the answer is True.
```

**Example 2**

```
Input: matrix = [[1,2],[2,2]]
Output: false
Explanation:
The diagonal "[1, 2]" has different elements.
```

## Method 1

```tex
【O(m * n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2024/12/04
 */
public class ToeplitzMatrix {
    public static boolean isToeplitzMatrix(int[][] matrix) {
        // Iterate through rows starting from the second row
        for (int i = 1; i < matrix.length; i++) {
            // Iterate through columns starting from the second column
            for (int j = 1; j < matrix[0].length; j++) {
                // Check if current element is equal to its top-left diagonal predecessor
                // If not equal, the matrix is not a Toeplitz matrix
                if(matrix[i][j] != matrix[i - 1][j - 1]){
                    return false;
                }
            }
        }

        // If all elements pass the diagonal consistency check
        return true;
    }

    public static void main(String[] args) {
        // Test Case 1: A valid Toeplitz matrix
        // In this matrix, each descending diagonal contains identical elements
        int[][] matrix1 = {
                {1, 2, 3, 4},
                {5, 1, 2, 3},
                {9, 5, 1, 2}
        };
        System.out.println("Test Case 1 is a Toeplitz matrix: " + isToeplitzMatrix(matrix1));

        // Test Case 2: A non-Toeplitz matrix
        // This matrix does not satisfy the Toeplitz matrix condition
        int[][] matrix2 = {
                {1, 2, 3},
                {4, 5, 6},
                {7, 8, 9}
        };
        System.out.println("Test Case 2 is a Toeplitz matrix: " + isToeplitzMatrix(matrix2));
    }
}

```
