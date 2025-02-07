---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "59. Spiral Matrix II"
date: "2025-02-07"
tags: Medium
categories:
  - "LeetCode Array"
---

- Given a positive integer `n`, generate an `n x n` `matrix` filled with elements from `1` to `n2` in spiral order.

**Example 1**

```
Input: n = 3
Output: [ [1,2,3],[8,9,4],[7,6,5 ] ]
```

**Example 2**

```
Input: n = 1
Output: [ [1] ]
```

## Method 1

```tex
【O(n²) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/02/07
 */
public class SpiralMatrixII {
    public static int[][] generateMatrix(int n) {
        // Initialize the result matrix
        int[][] matrix = new int[n][n];

        // Define four pointers to track the boundaries
        int left = 0;      // leftmost column not yet filled
        int right = n - 1; // rightmost column not yet filled
        int top = 0;       // topmost row not yet filled
        int bottom = n - 1;// bottommost row not yet filled

        // Start with 1 and go up to n^2
        int num = 1;

        // Continue until all numbers from 1 to n^2 are placed
        while (num <= n * n) {
            // Fill top row from left to right
            for (int i = left; i <= right; i++) {
                matrix[top][i] = num++;
            }
            top++; // Move top boundary down

            // Fill rightmost column from top to bottom
            for (int i = top; i <= bottom; i++) {
                matrix[i][right] = num++;
            }
            right--; // Move right boundary left

            // Fill bottom row from right to left
            for (int i = right; i >= left; i--) {
                matrix[bottom][i] = num++;
            }
            bottom--; // Move bottom boundary up

            // Fill leftmost column from bottom to top
            for (int i = bottom; i >= top; i--) {
                matrix[i][left] = num++;
            }
            left++; // Move left boundary right
        }

        return matrix;
    }
    
    public static void main(String[] args) {
        // Test Case 1: 3x3 matrix
        int[][] result1 = generateMatrix(3);
        System.out.println("Test Case 1:");
        printMatrix(result1);

        // Test Case 2: 1x1 matrix
        int[][] result2 = generateMatrix(1);
        System.out.println("Test Case 2:");
        printMatrix(result2);
    }
    
    public static void printMatrix(int[][] matrix) {
        for (int[] row : matrix) {
            for (int num : row) {
                System.out.print(num + " ");
            }
            System.out.println();
        }
    }
}

```





