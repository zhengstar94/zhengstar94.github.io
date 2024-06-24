---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "48.Rotate Image"
date: "2024-04-04"
categories:
  - "LeetCode MathGeometry"
---

# 48. Rotate Image

- You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).
- You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example 1**

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
```

**Example 2**

```
Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

## Method 1

```tex
【O(n^2) time | O(1) space】
```

```java
package Leetcode.MathGeometry;

/**
 * @author zhengstars
 * @date 2024/06/03
 */
public class RotateImage {
    /**
     * Rotates the given n x n 2D matrix by 90 degrees clockwise.
     * The transformation is performed in-place without allocating additional space for another matrix.
     *
     * @param matrix The n x n 2D matrix to be rotated.
     */
    public static void rotate(int[][] matrix) {
        int n = matrix.length;

        // Step 1: Transpose the matrix
        // Transpose the matrix by swapping matrix[i][j] with matrix[j][i]
        // This changes rows into columns.
        // For example, the matrix:
        // 1 2 3
        // 4 5 6
        // 7 8 9
        // Will become:
        // 1 4 7
        // 2 5 8
        // 3 6 9
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // Step 2: Reverse each row
        // Reverse each row by swapping the elements at the start with the elements at the end.
        // For example, the matrix:
        // 1 4 7
        // 2 5 8
        // 3 6 9
        // Will become:
        // 7 4 1
        // 8 5 2
        // 9 6 3
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n / 2; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][n - 1 - j];
                matrix[i][n - 1 - j] = temp;
            }
        }
    }

    public static void main(String[] args) {
        int[][] matrix1 = {
                {1, 2, 3},
                {4, 5, 6},
                {7, 8, 9}
        };
        rotate(matrix1);
        printMatrix(matrix1); // Expected output: [[7,4,1],[8,5,2],[9,6,3]]

        int[][] matrix2 = {
                {5, 1, 9, 11},
                {2, 4, 8, 10},
                {13, 3, 6, 7},
                {15, 14, 12, 16}
        };
        rotate(matrix2);
        printMatrix(matrix2); // Expected output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
    }

    private static void printMatrix(int[][] matrix) {
        for (int[] row : matrix) {
            for (int val : row) {
                System.out.print(val + " ");
            }
            System.out.println();
        }
    }
}

```

