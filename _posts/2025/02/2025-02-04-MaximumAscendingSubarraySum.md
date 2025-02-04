---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1800. Maximum Ascending Subarray Sum"
date: "2025-02-04"
tags: Easy
categories:
  - "LeetCode Array"
---


- Given an array of positive integers `nums`, return the *maximum possible sum of an **ascending** subarray in* `nums`.
- A subarray is defined as a contiguous sequence of numbers in an array.
- A subarray `[numsl, numsl+1, ..., numsr-1, numsr]` is **ascending** if for all `i` where `l <= i < r`, `numsi  < numsi+1`. Note that a subarray of size `1` is **ascending**.

**Example 1**

```
Input: nums = [10,20,30,5,10,50]
Output: 65
Explanation: [5,10,50] is the ascending subarray with the maximum sum of 65.
```

**Example 2**

```
Input: nums = [10,20,30,40,50]
Output: 150
Explanation: [10,20,30,40,50] is the ascending subarray with the maximum sum of 150.
```

**Example 3**

```
Input: nums = [12,17,15,13,10,11,12]
Output: 33
Explanation: [10,11,12] is the ascending subarray with the maximum sum of 33.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/02/04
 */
public class MaximumAscendingSubarraySum {
    
    public static int maxAscendingSum(int[] nums) {
        // Handle edge cases: null array or empty array
        if (nums == null || nums.length == 0) {
            return 0;
        }

        // Initialize variables:
        // maxSum: keeps track of the maximum sum found so far
        // currentSum: keeps track of the current ascending sequence sum
        int maxSum = nums[0];
        int currentSum = nums[0];

        // Iterate through the array starting from second element
        for (int i = 1; i < nums.length; i++) {
            // If current element is greater than previous element
            // Continue building the ascending sequence
            if (nums[i] > nums[i - 1]) {
                currentSum += nums[i];
            } else {
                // If sequence breaks, start a new sequence from current element
                currentSum = nums[i];
            }

            // Update maxSum if currentSum is greater
            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Array with mixed ascending and non-ascending sequences
        int[] nums1 = {10,20,30,5,10,50};
        System.out.println("Test Case 1 Result: " + maxAscendingSum(nums1)); // Expected output: 65

        // Test Case 2: Completely ascending array
        int[] nums2 = {10,20,30,40,50};
        System.out.println("Test Case 2 Result: " + maxAscendingSum(nums2)); // Expected output: 150

        // Test Case 3: Array with multiple small ascending sequences
        int[] nums3 = {12,17,15,13,10,11,12};
        System.out.println("Test Case 3 Result: " + maxAscendingSum(nums3)); // Expected output: 33

        // Test Case 4: Array with single element
        int[] nums4 = {5};
        System.out.println("Test Case 4 Result: " + maxAscendingSum(nums4)); // Expected output: 5
    }
}

```





