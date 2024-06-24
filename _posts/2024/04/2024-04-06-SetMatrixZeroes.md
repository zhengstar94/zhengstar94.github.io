---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "73.Set Matrix Zeroes"
date: "2024-04-06"
categories:
  - "LeetCode MathGeometry"
---

# 73. Set Matrix Zeroes

- Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

  You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1**

```
Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
Output: [[1,0,1],[0,0,0],[1,0,1]]
```

**Example 2**

```
Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```

## Method 1

```tex
【O(n*m) time | O(1) space】
```

```java
package Leetcode.MathGeometry;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/06/06
 */
public class SetMatrixZeroes {

    /**
     * Sets the matrix cells to zero if their row or column contains at least one zero.
     *
     * @param matrix The input matrix.
     */
    public static void setZeroes(int[][] matrix) {
        boolean rowZero = false; // Flag to indicate if the first row needs to be zeroed
        boolean colZero = false; // Flag to indicate if the first column needs to be zeroed

        int rows = matrix.length; // Number of rows in the matrix
        int cols = matrix[0].length; // Number of columns in the matrix

        // Check if the first row contains zero
        for (int j = 0; j < cols; j++) {
            if (matrix[0][j] == 0) {
                rowZero = true;
                break;
            }
        }

        // Check if the first column contains zero
        for (int i = 0; i < rows; i++) {
            if (matrix[i][0] == 0) {
                colZero = true;
                break;
            }
        }

        // Use the first row and column to mark zero positions
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {
                if (matrix[i][j] == 0) {
                    matrix[0][j] = 0; // Mark the first element of the column
                    matrix[i][0] = 0; // Mark the first element of the row
                }
            }
        }

        // Set matrix cells to zero based on first row markers
        for (int i = 1; i < rows; i++) {
            if (matrix[i][0] == 0) {
                for (int j = 1; j < cols; j++) {
                    matrix[i][j] = 0; // Zero out the entire row
                }
            }
        }

        // Set matrix cells to zero based on first column markers
        for (int j = 1; j < cols; j++) {
            if (matrix[0][j] == 0) {
                for (int i = 1; i < rows; i++) {
                    matrix[i][j] = 0; // Zero out the entire column
                }
            }
        }

        // Finally, set the first row to zero if needed
        if (rowZero) {
            for (int j = 0; j < cols; j++) {
                matrix[0][j] = 0;
            }
        }

        // Finally, set the first column to zero if needed
        if (colZero) {
            for (int i = 0; i < rows; i++) {
                matrix[i][0] = 0;
            }
        }
    }

    public static void main(String[] args) {

        // Test case 1
        int[][] matrix1 = {
                {1, 1, 1},
                {1, 0, 1},
                {1, 1, 1}
        };
        setZeroes(matrix1);
        System.out.println(Arrays.deepToString(matrix1)); // Output: [[1, 0, 1], [0, 0, 0], [1, 0, 1]]

        // Test case 2
        int[][] matrix2 = {
                {0, 1, 2, 0},
                {3, 4, 5, 2},
                {1, 3, 1, 5}
        };
        setZeroes(matrix2);
        System.out.println(Arrays.deepToString(matrix2)); // Output: [[0, 0, 0, 0], [0, 4, 5, 0], [0, 3, 1, 0]]
    }
}
```

