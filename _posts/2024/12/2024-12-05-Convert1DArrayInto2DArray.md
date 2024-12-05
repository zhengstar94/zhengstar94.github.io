---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2022. Convert 1D Array Into 2D Array"
date: "2024-12-05"
tags: Easy
categories:
  - "LeetCode Array"
---

- You are given a **0-indexed** 1-dimensional (1D) integer array `original`, and two integers, `m` and `n`. You are tasked with creating a 2-dimensional (2D) array with ` m` rows and `n` columns using **all** the elements from `original`.
- The elements from indices `0` to `n - 1` (**inclusive**) of `original` should form the first row of the constructed 2D array, the elements from indices `n` to `2 * n - 1` (**inclusive**) should form the second row of the constructed 2D array, and so on.
- Return *an* `m x n` *2D array constructed according to the above procedure, or an empty 2D array if it is impossible*.

**Example 1**

```
Input: original = [1,2,3,4], m = 2, n = 2
Output: [[1,2],[3,4]]
Explanation: The constructed 2D array should contain 2 rows and 2 columns.
The first group of n=2 elements in original, [1,2], becomes the first row in the constructed 2D array.
The second group of n=2 elements in original, [3,4], becomes the second row in the constructed 2D array.
```

**Example 2**

```
Input: original = [1,2,3], m = 1, n = 3
Output: [[1,2,3]]
Explanation: The constructed 2D array should contain 1 row and 3 columns.
Put all three elements in original into the first row of the constructed 2D array.
```

**Example 3**

```
Input: original = [1,2], m = 1, n = 1
Output: []
Explanation: There are 2 elements in original.
It is impossible to fit 2 elements in a 1x1 2D array, so return an empty 2D array.
```

## Method 1

```tex
【O(m * n) time | O(m * n) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2024/12/05
 */
public class Convert1DArrayInto2DArray {
    public static int[][] construct2DArray(int[] original, int m, int n) {
        // Check if the size of the original array matches the required dimensions.
        if (original.length != m * n) {
            return new int[0][0]; // Return an empty 2D array if the dimensions do not match.
        }

        // Initialize a 2D array with m rows and n columns.
        int[][] result = new int[m][n];

        // Fill the 2D array using elements from the original 1D array.
        for (int i = 0; i < m; i++) { // Loop through each row.
            for (int j = 0; j < n; j++) { // Loop through each column.
                // Map the 1D array index to the 2D array:
                // - i * n calculates the starting index for the current row in the 1D array.
                // - j adds the column offset within the current row.
                // Example: For i = 1 (2nd row) and n = 3 (3 columns), the row starts at index 3 in the 1D array.
                // Adding j = 0, 1, 2 fills the 2nd row of the 2D array.
                result[i][j] = original[i * n + j];
            }
        }

        return result; // Return the constructed 2D array.
    }

    public static void main(String[] args) {
        // Test Case 1: Valid input with 2 rows and 3 columns.
        int[] original1 = {1, 2, 3, 4, 5, 6};
        int m1 = 2, n1 = 3;
        System.out.println("Test Case 1:");
        print2DArray(construct2DArray(original1, m1, n1));

        // Test Case 2: Invalid input where the size does not match m * n.
        int[] original2 = {1, 2, 3, 4};
        int m2 = 3, n2 = 2;
        System.out.println("Test Case 2:");
        print2DArray(construct2DArray(original2, m2, n2));

        // Test Case 3: Edge case with 1 row and 4 columns.
        int[] original3 = {1, 2, 3, 4};
        int m3 = 1, n3 = 4;
        System.out.println("Test Case 3:");
        print2DArray(construct2DArray(original3, m3, n3));

        // Test Case 4: Edge case with 4 rows and 1 column.
        int[] original4 = {1, 2, 3, 4};
        int m4 = 4, n4 = 1;
        System.out.println("Test Case 4:");
        print2DArray(construct2DArray(original4, m4, n4));

        // Test Case 5: Empty input.
        int[] original5 = {};
        int m5 = 0, n5 = 0;
        System.out.println("Test Case 5:");
        print2DArray(construct2DArray(original5, m5, n5));
    }

    public static void print2DArray(int[][] array) {
        if (array.length == 0) { // Check if the array is empty.
            System.out.println("[]");
            return;
        }
        for (int[] row : array) { // Iterate through each row and print it.
            System.out.println(Arrays.toString(row));
        }
    }
}

```





