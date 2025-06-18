---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2966. Divide Array Into Arrays With Max Difference"
date: "2025-06-18"
tags: Medium
categories:
  - "LeetCode Greedy"
---


- You are given an integer array `nums` of size `n` where `n` is a multiple of 3 and a positive integer `k`.
- Divide the array `nums` into `n / 3` arrays of size **3** satisfying the following condition:
  - The difference between **any** two elements in one array is **less than or equal** to `k`.
- Return a **2D** array containing the arrays. If it is impossible to satisfy the conditions, return an empty array. And if there are multiple answers, return **any** of them.

**Example 1**

```
Input: nums = [1,3,4,8,7,9,3,5,1], k = 2

Output: [[1,1,3],[3,4,5],[7,8,9]]

Explanation:

The difference between any two elements in each array is less than or equal to 2.
```

**Example 2**

```
Input: nums = [2,4,2,2,5,2], k = 2

Output: []

Explanation:

Different ways to divide nums into 2 arrays of size 3 are:

[[2,2,2],[2,4,5]] (and its permutations)
[[2,2,4],[2,2,5]] (and its permutations)
Because there are four 2s there will be an array with the elements 2 and 5 no matter how we divide it. since 5 - 2 = 3 > k, the condition is not satisfied and so there is no valid division.
```

**Example 3**

```
Input: nums = [4,2,9,8,2,12,7,12,10,5,8,5,5,7,9,2,5,11], k = 14

Output: [[2,2,12],[4,8,5],[5,9,7],[7,8,5],[5,9,10],[11,12,2]]

Explanation:

The difference between any two elements in each array is less than or equal to 14.
```

## Method 1

```tex
【O(nlog(n)) time | O(n) space】
```

```java
package Leetcode.Greedy;

import java.util.Arrays;

/**
 * @Author zhengxingxing
 * @Date 2025/06/18
 */
public class DivideArrayIntoArraysWithMaxDifference {
    public static int[][] divideArray(int[] nums, int k) {
        // Step 1: Sort the array so that close values are adjacent
        Arrays.sort(nums);

        int n = nums.length;

        // Step 2: Prepare the result array of size n / 3
        int[][] ans = new int[n / 3][];

        // Step 3: Iterate over the sorted array in chunks of 3 elements
        for (int i = 2; i < n; i += 3) {
            // Step 4: Check if the current group of 3 elements satisfies the condition
            // Since the array is sorted, nums[i] is the largest, nums[i - 2] is the smallest
            // If the difference between them is greater than k, return an empty array
            if (nums[i] - nums[i - 2] > k) {
                return new int[][]{};
            }

            // Step 5: Otherwise, construct a valid group of 3 elements
            // and assign it to the answer array
            ans[i / 3] = new int[]{nums[i - 2], nums[i - 1], nums[i]};
        }

        // Step 6: Return the valid grouped array
        return ans;
    }

    public static void print2DArray(int[][] arr) {
        if (arr.length == 0) {
            System.out.println("[]");
            return;
        }
        for (int[] row : arr) {
            System.out.println(Arrays.toString(row));
        }
    }

    public static void main(String[] args) {
        int[] nums1 = {1, 3, 4, 8, 7, 9, 3, 5, 1};
        int k1 = 2;
        System.out.println("Test case 1:");
        print2DArray(divideArray(nums1, k1));
        // Expected output: [[1,1,3], [3,4,5], [7,8,9]]

        int[] nums2 = {2, 4, 2, 2, 5, 2};
        int k2 = 2;
        System.out.println("Test case 2:");
        print2DArray(divideArray(nums2, k2));
        // Expected output: [] (no valid grouping possible)

        int[] nums3 = {4, 2, 9, 8, 2, 12, 7, 12, 10, 5, 8, 5, 5, 7, 9, 2, 5, 11};
        int k3 = 14;
        System.out.println("Test case 3:");
        print2DArray(divideArray(nums3, k3));
        // Expected output: One of many valid groupings where all 3-element groups have max diff <= 14
    }
}

```





