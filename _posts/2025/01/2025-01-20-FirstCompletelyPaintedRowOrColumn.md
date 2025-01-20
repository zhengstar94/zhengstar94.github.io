---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2661. First Completely Painted Row or Column"
date: "2025-01-20"
tags: Medium
categories:
  - "LeetCode Array"
---


- You are given a **0-indexed** integer array `arr`, and an `m x n` integer **matrix** `mat`. `arr` and `mat` both contain **all** the integers in the range `[1, m * n]`.
- Go through each index `i` in `arr` starting from index `0` and paint the cell in `mat` containing the integer `arr[i]`.
- Return *the smallest index* `i` *at which either a row or a column will be completely painted in* `mat`.

**Example 1**

```
Input: arr = [1,3,4,2], mat = [[1,4],[2,3]]
Output: 2
Explanation: The moves are shown in order, and both the first row and second column of the matrix become fully painted at arr[2].
```

**Example 2**

```
Input: arr = [2,8,7,4,1,3,5,6,9], mat = [[3,2,5],[1,4,6],[8,7,9]]
Output: 3
Explanation: The second column becomes fully painted at arr[3].
```

## Method 1

```tex
【O(n*m) time | O(n*m) space】
```

```java
package Leetcode.Array;

import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2025/01/20
 */
public class FirstCompletelyPaintedRowOrColumn {

    // Function to find the first index in `arr` such that a row or a column in the matrix `mat` is completely filled
    public static int firstCompleteIndex(int[] arr, int[][] mat) {
        // Get the number of rows in the matrix
        int m = mat.length;
        // Get the number of columns in the matrix
        int n = mat[0].length;

        // Create a hash map to store the position (row and column) of each number in the matrix
        Map<Integer, int[]> numToPos = new HashMap<>();

        // Populate the hash map:
        // Loop through all rows (i)
        for (int i = 0; i < m; i++) {
            // Loop through all columns (j)
            for (int j = 0; j < n; j++) {
                // Add the current matrix element as the key, and its position [row, column] as the value
                numToPos.put(mat[i][j], new int[]{i, j});
            }
        }

        // Create two arrays to track the count of painted cells in each row and each column
        int[] rowCount = new int[m]; // Keeps track of how many cells are painted in each row
        int[] colCount = new int[n]; // Keeps track of how many cells are painted in each column

        // Loop through the array `arr` in the given order
        for (int i = 0; i < arr.length; i++) {
            // Get the position of the current number from the hash map
            int[] pos = numToPos.get(arr[i]);
            int row = pos[0]; // Row index of the current number in `mat`
            int col = pos[1]; // Column index of the current number in `mat`

            // Increment the count for the corresponding row and column
            rowCount[row]++;
            colCount[col]++;

            // Check if the current row has been completely painted
            // OR if the current column has been completely painted
            if (rowCount[row] == n || colCount[col] == m) {
                return i; // If so, return the current index from `arr`
            }
        }

        // If no row or column is completely painted, return the last possible index
        return arr.length - 1;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] arr1 = {1, 3, 4, 2};
        int[][] mat1 = {{1, 4}, {2, 3}};
        // Expected output: 2, as the first completely painted row or column (row 0) is done on the 3rd element (index 2)
        System.out.println("Test Case 1 Result: " + firstCompleteIndex(arr1, mat1));

        // Test case 2
        int[] arr2 = {2, 8, 7, 4, 1, 3, 5, 6, 9};
        int[][] mat2 = {{3, 2, 5}, {1, 4, 6}, {8, 7, 9}};
        // Expected output: 3, as the first completely painted column (column 1) is done on the 4th element (index 3)
        System.out.println("Test Case 2 Result: " + firstCompleteIndex(arr2, mat2));
    }
}

```





