---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "74. Search a 2D Matrix"
date: "2024-01-12"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 74. Search a 2D Matrix [Medium]

- Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:
  - Integers in each row are sorted from left to right.
  - The first integer of each row is greater than the last integer of the previous row.

**Example 1**

```
Input: 
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3

Output: true
```

**Example 2**

```
Input: 
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13

Output: false
```

## Method 1

```tex
【O(log(n*m))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2024/01/26
 */
public class SearchA2DMatrix {

    public static boolean searchMatrix(int[][] matrix, int target) {
        // Check for empty matrix
        if (matrix == null || matrix.length == 0) {
            return false;
        }

        int left = 0;  //Define leftmost boundary of the search array
        int right = matrix.length * matrix[0].length; // Define rightmost boundary of the search array
        int cols = matrix[0].length; // Get number of columns in matrix

        // Proceed with Binary Search
        while (left < right) {
            int mid = left + (right - left) / 2; // Calculate mid
            // Using integer division to calculate row(mid/cols) and column(mid%cols) of mid's position
            if (matrix[mid / cols][mid % cols] == target) {
                return true; // Return true if target is found
            } else if (matrix[mid / cols][mid % cols] > target) {
                right = mid; // If target is smaller than value at mid, search in left half
            } else {
                left = mid + 1; // If target is larger than value at mid, search in right half
            }
        }
        return false; // If target is not found, return false
    }

    public static void main(String[] args) {
        int[][] matrix = {
                {1,   3,  5,  7},
                {10, 11, 16, 20},
                {23, 30, 34, 50}
        };
        // Test case 1: Search for 3 (which is present in matrix) should return true
        System.out.println(searchMatrix(matrix, 3));
        // Test case 2: Search for 13 (which is not present in matrix) should return false
        System.out.println(searchMatrix(matrix, 13));
    }

}

```


