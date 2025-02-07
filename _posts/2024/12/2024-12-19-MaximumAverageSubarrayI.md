---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "643. Maximum Average Subarray I"
date: "2024-12-20"
tags: Easy SlideWindow
categories:
  - "LeetCode SlideWindow"
---

- You are given an integer array `nums` consisting of `n` elements, and an integer `k`.
- Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return *this value*. Any answer with a calculation error less than `10-5` will be accepted.


**Example 1**

```
Input: nums = [1,12,-5,-6,50,3], k = 4
Output: 12.75000
Explanation: Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75
```

**Example 2**

```
Input: nums = [5], k = 1
Output: 5.00000
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2024/12/20
 */
public class MaximumAverageSubarrayI {
    public static double findMaxAverage(int[] nums, int k) {
        int n = nums.length;
        if (n == 0) {
          return 0;
        }
  
        // Running sum of current window
        int subArraySum = 0;
        // Initialize max sum with the minimum value
        int maxSum = Integer.MIN_VALUE;
  
        for (int i = 0; i < n; i++) {
          // Add current element to window sum
          subArraySum += nums[i];
  
          // Skip until we have k elements in our window
          if (i < k - 1) {
            continue;
          }
  
          // Update max sum
          maxSum = Math.max(maxSum, subArraySum);
  
          // Remove the leftmost element from window sum
          subArraySum -= nums[i - k + 1];
        }
  
        // Return the maximum average
        return (1.0 * maxSum) / k;
    }

    public static void main(String[] args) {

        // Test Case 1: Regular array with positive numbers
        int[] nums1 = {1, 12, -5, -6, 50, 3};
        int k1 = 4;
        System.out.println("Test Case 1: " + findMaxAverage(nums1, k1));  // Expected: 12.75

        // Test Case 2: Array with all same numbers
        int[] nums2 = {5, 5, 5, 5, 5};
        int k2 = 3;
        System.out.println("Test Case 2: " + findMaxAverage(nums2, k2));  // Expected: 5.0

        // Test Case 3: Array with negative numbers
        int[] nums3 = {-1, -2, -3, -4, -5};
        int k3 = 2;
        System.out.println("Test Case 3: " + findMaxAverage(nums3, k3));  // Expected: -1.5

        // Test Case 4: Array where k equals array length
        int[] nums4 = {1, 2, 3};
        int k4 = 3;
        System.out.println("Test Case 4: " + findMaxAverage(nums4, k4));  // Expected: 2.0
    }
}

```





