---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1901. Find a Peak Element II"
date: "2025-05-28"
tags: Medium
categories:
  - "LeetCode BinarySearch"
---


- A **peak** element in a 2D grid is an element that is **strictly greater** than all of its **adjacent** neighbors to the left, right, top, and bottom.
- Given a **0-indexed** `m x n` matrix `mat` where **no two adjacent cells are equal**, find **any** peak element `mat[i][j]` and return *the length 2 array* `[i,j]`.
- You may assume that the entire matrix is surrounded by an **outer perimeter** with the value `-1` in each cell.
- You must write an algorithm that runs in `O(m log(n))` or `O(n log(m))` time.

**Example 1**

```
Input: mat = [ [ 1,4],[3,2 ] ]
Output: [0,1]
Explanation: Both 3 and 4 are peak elements so [1,0] and [0,1] are both acceptable answers.
```

**Example 2**

```
Input: mat = [ [ 10,20,15],[21,30,14],[7,16,32 ] ]
Output: [1,1]
Explanation: Both 30 and 32 are peak elements so [1,1] and [2,2] are both acceptable answers.
```

## Method 1

```tex
【O(nlogm) time | O(1) space】
```

```java
package Leetcode.BinarySearch;

/**
 * @Author zhengxingxing
 * @Date 2025/05/28
 */
public class FindAPeakElementII {

    public static int[] findPeakGrid(int[][] mat) {
        // Initialize binary search boundaries for rows
        int left = 0;                    // Start from first row
        int right = mat.length - 1;      // End at last row

        // Perform binary search on rows
        while (left < right) {
            // Find middle row to avoid integer overflow
            int i = left + (right - left) / 2;

            // Find the column index of maximum element in current row i
            // This is crucial: the max element in a row is already a peak
            // in horizontal direction (left-right), we only need to check vertical
            int j = indexOfMax(mat[i]);

            // Compare current max element with the element directly below it
            // This comparison determines which direction to search next
            if (mat[i][j] > mat[i + 1][j]) {
                // Current element is greater than element below
                // This means:
                // 1. If current element is also > element above, it's a peak
                // 2. If current element <= element above, peak exists above
                // In both cases, peak exists in current row or above
                right = i;  // Search in upper half (including current row)
            } else {
                // Current element <= element below
                // This means there's a larger element below, so peak must be below
                // We can safely exclude current row and search below
                left = i + 1;  // Search in lower half (excluding current row)
            }
        }

        // When left == right, we've found the row containing a peak
        // Return the coordinates: [row_index, column_index_of_max_in_that_row]
        return new int[]{left, indexOfMax(mat[left])};
    }
    
    private static int indexOfMax(int[] a) {
        int idx = 0;  // Initialize with first element's index

        // Iterate through the array to find maximum element's index
        for (int i = 0; i < a.length; i++) {
            // If current element is greater than previously found max
            if (a[i] > a[idx]) {
                idx = i;  // Update max element's index
            }
        }

        return idx;  // Return index of maximum element
    }
    
    public static void main(String[] args) {
        System.out.println("=== LeetCode 1901: Find a Peak Element II ===\n");

        // Test Case 1: Example from LeetCode
        System.out.println("Test Case 1:");
        int[][] mat1 = { { 1, 4}, {3, 2 } };
        System.out.println("Input matrix:");
        printMatrix(mat1);
        int[] result1 = findPeakGrid(mat1);
        System.out.println("Peak found at: [" + result1[0] + ", " + result1[1] + "]");
        System.out.println("Peak value: " + mat1[result1[0]][result1[1]]);
        System.out.println("Verification: " + verifyPeak(mat1, result1[0], result1[1]));
        System.out.println();

        // Test Case 2: Example from LeetCode
        System.out.println("Test Case 2:");
        int[][] mat2 = { { 10, 20, 15}, {21, 30, 14}, {7, 16, 32 } };
        System.out.println("Input matrix:");
        printMatrix(mat2);
        int[] result2 = findPeakGrid(mat2);
        System.out.println("Peak found at: [" + result2[0] + ", " + result2[1] + "]");
        System.out.println("Peak value: " + mat2[result2[0]][result2[1]]);
        System.out.println("Verification: " + verifyPeak(mat2, result2[0], result2[1]));
        System.out.println();
    }
    
    private static void printMatrix(int[][] mat) {
        for (int[] row : mat) {
            System.out.print("[");
            for (int j = 0; j < row.length; j++) {
                System.out.printf("%3d", row[j]);
                if (j < row.length - 1) {
                    System.out.print(", ");
                }
            }
            System.out.println("]");
        }
    }


    private static boolean verifyPeak(int[][] mat, int row, int col) {
        int m = mat.length;
        int n = mat[0].length;
        int current = mat[row][col];

        // Check all four directions
        // Up
        if (row > 0 && mat[row - 1][col] >= current) {
            return false;
        }
        // Down
        if (row < m - 1 && mat[row + 1][col] >= current) {
            return false;
        }
        // Left
        if (col > 0 && mat[row][col - 1] >= current) {
            return false;
        }
        // Right
        if (col < n - 1 && mat[row][col + 1] >= current) {
            return false;
        }

        return true;  // All adjacent elements are smaller
    }
}
```





