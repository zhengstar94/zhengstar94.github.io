---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "378. Kth Smallest Element in a Sorted Matrix"
date: "2025-05-21"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.KthElement"
---

- Given an `n x n` `matrix` where each of the rows and columns is sorted in ascending order, return *the* `kth` *smallest element in the matrix*.
- Note that it is the `kth` smallest element **in the sorted order**, not the `kth` **distinct** element.
- You must find a solution with a memory complexity better than `O(n^2)`.

**Example 1**

```
Input: matrix = [ [ 1,5,9],[10,11,13],[12,13,15 ] ], k = 8
Output: 13
Explanation: The elements in the matrix are [1,5,9,10,11,12,13,13,15], and the 8th smallest number is 13
```

**Example 2**

```
Input: matrix = [ [ -5 ] ], k = 1
Output: -5
```

## Method 1

```tex
【O(n × log(max-min)) time | O(1) space】
```

```java
package Leetcode.BinarySearch.KthElement;

/**
 * author: zhengxingxing
 * date: 2025/05/21
 */
public class KthSmallestElementInASortedMatrix {

    public static int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;

        int left = matrix[0][0];         // The smallest possible value in the matrix
        int right = matrix[n - 1][n - 1]; // The largest possible value in the matrix

        // Binary search on value range
        while (left < right) {
            int mid = left + (right - left) / 2;  // Prevents potential overflow

            // Count how many elements in the matrix are ≤ mid
            int count = countLessOrEqual(matrix, mid);

            if (count < k) {
                // If there are fewer than k elements ≤ mid, then the target must be in the right half
                left = mid + 1;
            } else {
                // Otherwise, mid is large enough (maybe too large), move left
                right = mid;
            }
        }

        // When left == right, we've found the smallest number such that at least k elements are ≤ it
        return left;
    }

    /**
     * Counts the number of elements in the matrix that are less than or equal to the given target.
     *
     * We take advantage of the sorted property:
     * - Start from the bottom-left corner.
     * - If the current number is ≤ target, then all numbers above it in that column are also ≤ target.
     * - If it's > target, move upward to smaller numbers.
     *
     * Example: For matrix[i][j]
     * - If matrix[i][j] ≤ target → count += i + 1, move right
     * - Else → move up
     *
     * Time Complexity: O(n), where n is number of rows/columns
     *
     * @param matrix the input sorted matrix
     * @param target the value to compare against
     * @return count of elements ≤ target
     */
    private static int countLessOrEqual(int[][] matrix, int target) {
        int n = matrix.length;
        int count = 0;

        int row = n - 1;  // Start from the bottom-left corner
        int col = 0;

        // Traverse while staying within matrix boundaries
        while (row >= 0 && col < n) {
            if (matrix[row][col] <= target) {
                // Since matrix[row][col] ≤ target, all elements above in this column (from row 0 to current row)
                // are also ≤ target due to sorted column. So we can add (row + 1) elements in one go.
                count += row + 1;

                // Move to next column (right)
                col++;
            } else {
                // If current element is > target, move up to smaller elements
                row--;
            }
        }

        return count;
    }

    /**
     * Test cases to validate the solution.
     */
    public static void main(String[] args) {
        // Test Case 1
        int[][] matrix1 = {
                {1, 5, 9},
                {10, 11, 13},
                {12, 13, 15}
        };
        int k1 = 8;
        System.out.println("Test Case 1:");
        System.out.println("Input: matrix = [ [ 1,5,9],[10,11,13],[12,13,15 ] ], k = 8");
        System.out.println("Output: " + kthSmallest(matrix1, k1));
        System.out.println("Expected: 13\n");

        // Test Case 2
        int[][] matrix2 = { { -5 } };
        int k2 = 1;
        System.out.println("Test Case 2:");
        System.out.println("Input: matrix = [ [ -5 ] ], k = 1");
        System.out.println("Output: " + kthSmallest(matrix2, k2));
        System.out.println("Expected: -5");
    }
}

```





