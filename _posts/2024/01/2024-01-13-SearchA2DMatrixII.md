---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "240. Search a 2D Matrix II"
date: "2024-01-13"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 240. Search a 2D Matrix II [Medium]

- Write an efficient algorithm that searches for a value `target` in an `m x n` integer matrix `matrix`. This matrix has the following properties:
  - Integers in each row are sorted in ascending from left to right.
  - Integers in each column are sorted in ascending from top to bottom.

**Example 1**

```
Input: matrix = 
[
[1,4,7,11,15],
[2,5,8,12,19],
[3,6,9,16,22],
[10,13,14,17,24],
[18,21,23,26,30]
], target = 5
Output: true
```

**Example 2**

```
Input: matrix = [
[1,4,7,11,15],
[2,5,8,12,19],
[3,6,9,16,22],
[10,13,14,17,24],
[18,21,23,26,30]
], target = 20
Output: false
```

## Method 1

```tex
【O(m+n)time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2024/02/17
 */
public class SearchA2DMatrixII {
    public static boolean searchMatrix(int[][] matrix, int target) {
        // Initiate row and column markers
        int row = 0;
        int col = matrix[0].length - 1;

        // As long as the row and column are within the boundary of the matrix
        while (row < matrix.length && col >= 0) {
            // If the target is to the left of the current element, move the column marker to the left
            if (matrix[row][col] > target) {
                col--;
            }
            // If the target is below the current element, move the row marker down
            else if (matrix[row][col] < target) {
                row++;
            }
            // If target is found
            else {
                return true;
            }
        }

        // If we've searched the whole matrix and haven't found the target, return false
        return false;
    }

    public static void main(String[] args) {
        // Define a matrix and a target value for testing
        int[][] matrix1 = { {1,4,7,11,15},{2,5,8,12,19},{3,6,9,16,22},{10,13,14,17,24},{18,21,23,26,30}};
        int target1 = 5;
        // Print out the test result
        System.out.println(searchMatrix(matrix1, target1));    // Output should be: true

        // Define another matrix and a target value for testing
        int[][] matrix2 = { {1,4,7,11,15},{2,5,8,12,19},{3,6,9,16,22},{10,13,14,17,24},{18,21,23,26,30}};
        int target2 = 20;
        // Print out the test result
        System.out.println(searchMatrix(matrix2, target2));    // Output should be: false
    }
}

```
