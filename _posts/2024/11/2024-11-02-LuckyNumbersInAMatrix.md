---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1380. Lucky Numbers in a Matrix"
date: "2024-11-02"
categories:
  - "LeetCode Array"
---

- Given an `m x n` matrix of **distinct** numbers, return *all **lucky numbers** in the matrix in **any** order*.
- A **lucky number** is an element of the matrix such that it is the minimum element in its row and maximum in its column.

**Example 1**

```
Input: matrix = [[3,7,8],[9,11,13],[15,16,17]]
Output: [15]
Explanation: 15 is the only lucky number since it is the minimum in its row and the maximum in its column.
```

**Example 2**

```
Input: matrix = [[1,10,4,2],[9,3,8,7],[15,16,17,12]]
Output: [12]
Explanation: 12 is the only lucky number since it is the minimum in its row and the maximum in its column.
```

**Example 3**

```
Input: matrix = [[7,8],[1,2]]
Output: [7]
Explanation: 7 is the only lucky number since it is the minimum in its row and the maximum in its column.
```

## Method 1

```tex
【O(m * n) time | O(1) space】
```

```java
package Leetcode.Array;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2024/11/02
 */
public class LuckyNumbersInAMatrix {
    public static List<Integer> luckyNumbers(int[][] matrix) {
        List<Integer> result = new ArrayList<>();

        // Iterate through each row of the matrix
        for (int i = 0; i < matrix.length; i++) {
            // Find the column index of minimum element in current row
            int col = findMinCol(i, matrix);

            // Check if this minimum element is also maximum in its column
            if (confirmCol(matrix[i][col], col, matrix)) {
                result.add(matrix[i][col]);
            }
        }

        return result;
    }

    private static int findMinCol(int row, int[][] matrix) {
        int minRow = 0;  // Column index of minimum element
        int min = Integer.MAX_VALUE;  // Current minimum value

        // Iterate through each column in the specified row
        for (int col = 0; col < matrix[0].length; col++) {
            // Update minimum value and its column index if current element is smaller
            if(matrix[row][col] < min){
                min = matrix[row][col];
                minRow = col;
            }
        }

        return minRow;
    }

    private static boolean confirmCol(int num, int col, int[][] matrix) {
        // Check each element in the specified column
        for (int i = 0; i < matrix.length; i++) {
            // If any element is greater than our number, return false
            if(matrix[i][col] > num){
                return false;
            }
        }
        // If no greater element found, return true
        return true;
    }

    public static void main(String[] args) {
        // Test case 1: Expected output [15]
        // Matrix where 15 is minimum in its row and maximum in its column
        int[][] matrix1 = {
                {3, 7, 8},
                {9, 11, 13},
                {15, 16, 17}
        };
        System.out.println("Test case 1: " + luckyNumbers(matrix1));

        // Test case 2: Expected output [12]
        // Matrix where 12 is minimum in its row and maximum in its column
        int[][] matrix2 = {
                {1, 10, 4, 2},
                {9, 3, 8, 7},
                {15, 16, 17, 12}
        };
        System.out.println("Test case 2: " + luckyNumbers(matrix2));

        // Test case 3: Expected output [7]
        // Small matrix where 7 is minimum in its row and maximum in its column
        int[][] matrix3 = {
                {7, 8},
                {1, 2}
        };
        System.out.println("Test case 3: " + luckyNumbers(matrix3));
    }
}
```





