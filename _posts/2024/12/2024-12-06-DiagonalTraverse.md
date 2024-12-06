---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "498. Diagonal Traverse"
date: "2024-12-06"
tags: Medium
categories:
  - "LeetCode Array"
---

- Given an `m x n` matrix `mat`, return *an array of all the elements of the array in a diagonal order*.

**Example 1**

```
Input: mat = [ [1,2,3],[4,5,6],[7,8,9] ]
Output: [1,2,4,7,5,3,6,8,9]
```

**Example 2**

```
Input: mat = [ [ 1,2],[3,4 ] ]
Output: [1,2,3,4]

```

## Method 1

```tex
【O(m * n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2024/12/06
 */
public class DiagonalTraverse {
    
    public static int[] findDiagonalOrder(int[][] mat) {
        // Number of rows in the matrix
        int m = mat.length;

        // Number of columns in the matrix
        int n = mat[0].length;

        // Result array to store diagonal traversal
        // Size is total number of elements in the matrix
        int[] result = new int[m * n];

        // Index to track current position in the result array
        int index = 0;

        // Total number of diagonals is (m + n - 1)
        // We iterate through each diagonal
        for (int i = 0; i < m + n - 1; i++) {
            // Even-indexed diagonals: Bottom-left to Top-right direction
            if (i % 2 == 0) {
                // Determine the starting point for the current diagonal

                // Row starting point calculation:
                // If current diagonal index is less than number of rows,
                // start from that row index
                // Otherwise, start from the last row
                int x = i < m ? i : m - 1;

                // Column starting point calculation:
                // If current diagonal index is less than number of rows,
                // start from first column (0)
                // Otherwise, calculate the starting column based on diagonal index
                int y = i < m ? 0 : i - m + 1;

                // Traverse the diagonal while within matrix bounds
                while (x >= 0 && y < n) {
                    // Add current matrix element to result
                    result[index++] = mat[x][y];

                    // Move upwards in the matrix
                    x--;

                    // Move to the right in the matrix
                    y++;
                }
            }
            // Odd-indexed diagonals: Top-right to Bottom-left direction
            else {
                // Determine the starting point for the current diagonal

                // Row starting point calculation:
                // If current diagonal index is less than number of columns,
                // start from first row
                // Otherwise, calculate the starting row based on diagonal index
                int x = i < n ? 0 : i - n + 1;

                // Column starting point calculation:
                // If current diagonal index is less than number of columns,
                // start from that column index
                // Otherwise, start from the last column
                int y = i < n ? i : n - 1;

                // Traverse the diagonal while within matrix bounds
                while (x < m && y >= 0) {
                    // Add current matrix element to result
                    result[index++] = mat[x][y];

                    // Move downwards in the matrix
                    x++;

                    // Move to the left in the matrix
                    y--;
                }
            }
        }

        // Return the array with diagonal traversal order
        return result;
    }

    /**
     * Utility method to print an array
     *
     * @param arr Input array to be printed
     */
    public static void printArray(int[] arr) {
        // Iterate through each element and print with space
        for (int num : arr) {
            System.out.print(num + " ");
        }
        // Print a new line after array elements
        System.out.println();
    }

    /**
     * Main method to demonstrate diagonal traversal with different matrix types
     *
     * @param args Command line arguments (not used)
     */
    public static void main(String[] args) {
        // Test Case 1: 3x3 Square Matrix
        int[][] mat1 = {
                {1, 2, 3},
                {4, 5, 6},
                {7, 8, 9}
        };
        System.out.println("3x3 Matrix Traversal Result:");
        int[] result1 = findDiagonalOrder(mat1);
        printArray(result1);

        // Test Case 2: 4x4 Square Matrix
        int[][] mat2 = {
                {1,  2,  3,  4},
                {5,  6,  7,  8},
                {9, 10, 11, 12},
                {13, 14, 15, 16}
        };
        System.out.println("4x4 Matrix Traversal Result:");
        int[] result2 = findDiagonalOrder(mat2);
        printArray(result2);

        // Test Case 3: Rectangular Matrix
        int[][] mat3 = {
                {1, 2, 3, 4},
                {5, 6, 7, 8},
                {9, 10, 11, 12}
        };
        System.out.println("3x4 Matrix Traversal Result:");
        int[] result3 = findDiagonalOrder(mat3);
        printArray(result3);
    }
}
```





