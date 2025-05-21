---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3355. Zero Array Transformation I"
date: "2025-05-20"
tags: Medium
categories:
  - "LeetCode Array"
---


- You are given an integer array `nums` of length `n` and a 2D array `queries`, where `queries[i] = [li, ri]`.
- For each `queries[i]`:
  - Select a subset of indices within the range `[li, ri]` in `nums`.
  - Decrement the values at the selected indices by 1.
- A **Zero Array** is an array where all elements are equal to 0.
- Return `true` if it is *possible* to transform `nums` into a **Zero Array** after processing all the queries sequentially, otherwise return `false`.

**Example 1**

```
Input: nums = [1,0,1], queries = [ [0,2] ]

Output: true

Explanation:

For i = 0:
Select the subset of indices as [0, 2] and decrement the values at these indices by 1.
The array will become [0, 0, 0], which is a Zero Array.
```

**Example 2**

```
Input: nums = [4,3,2,1], queries = [ [1,3],[0,2] ]

Output: false

Explanation:

For i = 0:
Select the subset of indices as [1, 2, 3] and decrement the values at these indices by 1.
The array will become [4, 2, 1, 0].
For i = 1:
Select the subset of indices as [0, 1, 2] and decrement the values at these indices by 1.
The array will become [3, 1, 0, 0], which is not a Zero Array.
```

## Method 1

```tex
【O(n + q) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/05/20
 */
public class ZeroArrayTransformationI {
    
    public static boolean isZeroArray(int[] nums, int[][] queries) {
        int n = nums.length;
        int[] diff = new int[n + 1]; // Difference array to simulate range updates efficiently

        // Apply each query as a range update: increment elements in the range [l, r] by 1
        for (int[] q : queries) {
            int l = q[0], r = q[1];
            diff[l]++;         // Start increment at index l
            diff[r + 1]--;     // Cancel the increment after index r
        }

        int sumD = 0; // Accumulator for the effective increments at each position

        for (int i = 0; i < n; i++) {
            sumD += diff[i]; // sumD represents how many times nums[i] is incremented

            // Since each increment operation is effectively a -1 to nums[i],
            // sumD also represents how much we are allowed to reduce nums[i] by.
            if (nums[i] > sumD) {
                // If nums[i] is still greater than the total reduction allowed,
                // it is impossible to reduce it to 0.
                return false;
            }
        }

        // If we never encountered a value that can't be zeroed out, return true
        return true;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {1, 0, 1};
        int[][] queries1 = { {0, 2} };
        boolean result1 = isZeroArray(nums1, queries1);
        System.out.println("Test case 1 result: " + result1); // Expected: true

        // Test case 2
        int[] nums2 = {4, 3, 2, 1};
        int[][] queries2 = { {1, 3}, {0, 2} };
        boolean result2 = isZeroArray(nums2, queries2);
        System.out.println("Test case 2 result: " + result2); // Expected: false

        // Additional test case
        int[] nums3 = {2, 3, 1, 4};
        int[][] queries3 = { {0, 1}, {1, 3}, {0, 2} };
        boolean result3 = isZeroArray(nums3, queries3);
        System.out.println("Test case 3 result: " + result3);
    }
}

```





