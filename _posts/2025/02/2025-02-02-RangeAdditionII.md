---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "598. Range Addition II"
date: "2025-02-02"
tags: Easy
categories:
  - "LeetCode Array"
---


- You are given an `m x n` matrix `M` initialized with all `0`'s and an array of operations `ops`, where `ops[i] = [ai, bi]` means `M[x][y]` should be incremented by one for all `0 <= x < ai` and `0 <= y < bi`
- Count and return *the number of maximum integers in the matrix after performing all the operations*.

**Example 1**

```
Input: m = 3, n = 3, ops = [ [2,2],[3,3] ]
Output: 4
Explanation: The maximum integer in M is 2, and there are four of it in M. So return 4.
```

**Example 2**

```
Input: m = 3, n = 3, ops = [ [2,2],[3,3],[3,3],[3,3],[2,2],[3,3],[3,3],[3,3],[2,2],[3,3],[3,3],[3,3] ]
Output: 4
```

**Example 3**

```
Input: m = 3, n = 3, ops = []
Output: 9
```

## Method 1

```tex
【O(k) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/02/02
 */
public class RangeAdditionII {
    
    public static int maxCount(int m, int n, int[][] ops) {
        // If operations array is empty or null, return the total area of matrix
        // as all elements will be 0 (maximum value) in this case
        if (ops == null || ops.length == 0) {
            return m * n;
        }

        // Initialize variables to track minimum rows and columns
        // The overlapping area will be determined by these minimums
        int minRow = m;
        int minCol = n;

        // Iterate through each operation to find the smallest affected area
        // This will be the area where all operations overlap
        for (int[] op : ops) {
            minRow = Math.min(minRow, op[0]);  // Find minimum row bound
            minCol = Math.min(minCol, op[1]);  // Find minimum column bound
        }

        // Return the area of overlap (number of maximum elements)
        // This is the product of minimum rows and columns
        return minRow * minCol;
    }

    
    public static void main(String[] args) {
        // Test Case 1: Basic case with two operations
        int m1 = 3, n1 = 3;
        int[][] ops1 = { { 2,2},{3,3 } };
        System.out.println("Test Case 1 Result: " + maxCount(m1, n1, ops1)); // Expected output: 4

        // Test Case 2: Multiple operations with same pattern
        int m2 = 3, n2 = 3;
        int[][] ops2 = { { 2,2},{3,3},{3,3},{3,3},{2,2},{3,3},{3,3},{3,3},{2,2},{3,3},{3,3},{3,3 } };
        System.out.println("Test Case 2 Result: " + maxCount(m2, n2, ops2)); // Expected output: 4

        // Test Case 3: Empty operations array
        int m3 = 3, n3 = 3;
        int[][] ops3 = {};
        System.out.println("Test Case 3 Result: " + maxCount(m3, n3, ops3)); // Expected output: 9
    }
}

```





