---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "54.Spiral Matrix"
date: "2024-04-05"
categories:
  - "LeetCode MathGeometry"
---

# 54. Spiral Matrix

- Given an `m x n` `matrix`, return *all elements of the* `matrix` *in spiral order*.

**Example 1**

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]
```

**Example 2**

```
Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

## Method 1

```tex
【O(n*m) time | O(n*m) space】
```

```java
package Leetcode.MathGeometry;

import java.util.ArrayList;
import java.util.List;

/**
 * Author: zhengstars
 * Date: 2024/06/05
 */
public class SpiralMatrix {
    /**
     * Method to return the elements of the matrix in spiral order.
     *
     * @param matrix The input 2D matrix.
     * @return The list of matrix elements in spiral order.
     */
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<>();

        // Check if the input matrix is empty or null.
        if (matrix == null || matrix.length == 0) {
            return result;
        }

        // Initialize the boundary pointers.
        int top = 0, bottom = matrix.length - 1;
        int left = 0, right = matrix[0].length - 1;

        // Continue the process while there are elements within the boundaries.
        while (top <= bottom && left <= right) {
            // Traverse from left to right on the current top boundary.
            // Add each element to the result list.
            for (int i = left; i <= right; i++) {
                result.add(matrix[top][i]);
            }
            top++; // Move the top boundary down.

            // Traverse from top to bottom on the current right boundary.
            // Add each element to the result list.
            for (int i = top; i <= bottom; i++) {
                result.add(matrix[i][right]);
            }
            right--; // Move the right boundary left.

            // Ensure there are still rows to traverse.
            if (top <= bottom) {
                // Traverse from right to left on the current bottom boundary.
                // Add each element to the result list.
                for (int i = right; i >= left; i--) {
                    result.add(matrix[bottom][i]);
                }
                bottom--; // Move the bottom boundary up.
            }

            // Ensure there are still columns to traverse.
            if (left <= right) {
                // Traverse from bottom to top on the current left boundary.
                // Add each element to the result list.
                for (int i = bottom; i >= top; i--) {
                    result.add(matrix[i][left]);
                }
                left++; // Move the left boundary right.
            }
        }

        // Return the result list containing the matrix elements in spiral order.
        return result;
    }

    /**
     * Main method to test the spiralOrder method with example inputs.
     *
     * @param args Command line arguments (not used).
     */
    public static void main(String[] args) {
        SpiralMatrix sm = new SpiralMatrix();

        int[][] matrix1 = {
                {1, 2, 3},
                {4, 5, 6},
                {7, 8, 9}
        };
        // Test case 1: Example input matrix1.
        // Expected output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
        System.out.println(sm.spiralOrder(matrix1));

        int[][] matrix2 = {
                {1, 2, 3, 4},
                {5, 6, 7, 8},
                {9, 10, 11, 12}
        };
        // Test case 2: Example input matrix2.
        // Expected output: [1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]
        System.out.println(sm.spiralOrder(matrix2));
    }
}
```

