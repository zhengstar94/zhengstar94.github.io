---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1749. Maximum Absolute Sum of Any Subarray"
date: "2025-02-26"
tags: Medium
categories:
  - "LeetCode Array"
---


- You are given an integer array `nums`. The **absolute sum** of a subarray `[numsl, numsl+1, ..., numsr-1, numsr]` is `abs(numsl + numsl+1 + ... + numsr-1 + numsr)`.
- Return *the **maximum** absolute sum of any **(possibly empty)** subarray of* `nums`.
- Note that `abs(x)` is defined as follows:
  - If `x` is a negative integer, then `abs(x) = -x`.
  - If `x` is a non-negative integer, then `abs(x) = x`.

**Example 1**

```
Input: nums = [1,-3,2,3,-4]
Output: 5
Explanation: The subarray [2,3] has absolute sum = abs(2+3) = abs(5) = 5.
```

**Example 2**

```
Input: nums = [2,-5,1,-4,3,-2]
Output: 8
Explanation: The subarray [-5,1,-4] has absolute sum = abs(-5+1-4) = abs(-8) = 8.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/02/26
 */
public class MaximumAbsoluteSumOfAnySubarray {

    
    public static int maxAbsoluteSum(int[] nums) {
        // Initialize variables to track maximum and minimum sums
        int maxSum = 0;
        int minSum = 0;
        // Variable to maintain running sum (prefix sum)
        int currSum = 0;

        // Iterate through the array once
        for (int num : nums) {
            // Add current number to running sum
            currSum += num;
            // Update maximum sum if current sum is larger
            maxSum = Math.max(maxSum, currSum);
            // Update minimum sum if current sum is smaller
            minSum = Math.min(minSum, currSum);
        }

        // Return the difference between maximum and minimum sums
        // This difference represents the maximum absolute sum possible
        return maxSum - minSum;
    }

    
    public static void main(String[] args) {
        // Test Case 1: Mixed positive and negative numbers
        int[] nums1 = {1, -3, 2, 3, -4};
        System.out.println("Test Case 1 Result: " + maxAbsoluteSum(nums1)); // Expected output: 5

        // Test Case 2: Another mixed case
        int[] nums2 = {2, -5, 1, -4, 3, -2};
        System.out.println("Test Case 2 Result: " + maxAbsoluteSum(nums2)); // Expected output: 8

        // Test Case 3: All positive numbers
        int[] nums3 = {1, 2, 3, 4};
        System.out.println("Test Case 3 Result: " + maxAbsoluteSum(nums3)); // Expected output: 10

        // Test Case 4: All negative numbers
        int[] nums4 = {-1, -2, -3, -4};
        System.out.println("Test Case 4 Result: " + maxAbsoluteSum(nums4)); // Expected output: 10
    }
}

```





