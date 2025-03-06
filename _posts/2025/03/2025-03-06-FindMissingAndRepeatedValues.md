---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2965. Find Missing and Repeated Values"
date: "2025-03-05"
tags: Medium
categories:
  - "LeetCode Math" 
---


- You are given a **0-indexed** 2D integer matrix `grid` of size `n * n` with values in the range `[1, n^2]`. Each integer appears **exactly once** except `a` which appears **twice** and `b` which is **missing**. The task is to find the repeating and missing numbers `a` and `b`.
- Return *a **0-indexed** integer array* `ans` *of size* `2` *where* `ans[0]` *equals to* `a` *and* `ans[1]` *equals to* `b`*.*

**Example 1**

```
Input: grid = [ [ 1,3],[2,2 ] ]
Output: [2,4]
Explanation: Number 2 is repeated and number 4 is missing so the answer is [2,4].
```

**Example 2**

```
Input: grid = [ [ 9,1,7],[8,9,2],[3,4,6 ] ]
Output: [9,5]
Explanation: Number 9 is repeated and number 5 is missing so the answer is [9,5].
```

## Method 1

```tex
【O(n^2) time | O(n^2) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/03/06
 */
public class FindMissingAndRepeatedValues {

    public static int[] findMissingAndRepeatedValues(int[][] grid) {
        // Get the dimension of the grid
        int n = grid.length;

        // Create array to store count of each number
        // Size is n*n+1 because numbers range from 1 to n^2
        int[] count = new int[n * n + 1];

        // Count frequency of each number in the grid
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                count[grid[i][j]]++;
            }
        }

        // Array to store result
        // result[0] will store repeated number
        // result[1] will store missing number
        int[] result = new int[2];

        // Check count array to find repeated and missing numbers
        // If count is 2, number is repeated
        // If count is 0, number is missing
        for (int i = 1; i <= n * n; i++) {
            if (count[i] == 2) {
                result[0] = i;    // Found repeated number
            } else if (count[i] == 0) {
                result[1] = i;    // Found missing number
            }
        }

        return result;
    }

    public static void main(String[] args) {
        // Test Case 1: 2x2 matrix
        // Expected output: [2,4] - 2 appears twice, 4 is missing
        int[][] grid1 = { { 1,3},{2,2 } };
        int[] result1 = findMissingAndRepeatedValues(grid1);
        System.out.println("Test Case 1 Result: [" + result1[0] + "," + result1[1] + "]");

        // Test Case 2: 3x3 matrix
        // Expected output: [9,5] - 9 appears twice, 5 is missing
        int[][] grid2 = { { 9,1,7},{8,9,2},{3,4,6 } };
        int[] result2 = findMissingAndRepeatedValues(grid2);
        System.out.println("Test Case 2 Result: [" + result2[0] + "," + result2[1] + "]");

        // Test Case 3: Additional test case
        // Expected output: [1,2] - 1 appears twice, 2 is missing
        int[][] grid3 = { { 1,1},{3,4 } };
        int[] result3 = findMissingAndRepeatedValues(grid3);
        System.out.println("Test Case 3 Result: [" + result3[0] + "," + result3[1] + "]");
    }
}

```





