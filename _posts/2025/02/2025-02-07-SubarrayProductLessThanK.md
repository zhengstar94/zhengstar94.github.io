---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "713. Subarray Product Less Than K"
date: "2025-02-07"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindowCountShortest"
---


- Given an array of integers `nums` and an integer `k`, return *the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than* `k`.

**Example 1**

```
Input: nums = [10,5,2,6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are:
[10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6]
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.
```

**Example 2**

```
Input: nums = [1,2,3], k = 0
Output: 0
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowCountShortest;

/**
 * @author zhengxingxing
 * @date 2025/02/07
 */
public class SubarrayProductLessThanK {

    public static int numSubarrayProductLessThanK(int[] nums, int k) {
        // Edge case: if k <= 1, no subarray can have product less than k
        // since all numbers in the array are positive
        if (k < 1) {
            return 0;
        }

        // Initialize variables for sliding window
        int count = 0;      // Counter for valid subarrays
        int product = 1;    // Current product of elements in window
        int left = 0;       // Left pointer of sliding window

        // Iterate through array with right pointer
        for (int right = 0; right < nums.length; right++) {
            // Include current element in product
            product *= nums[right];

            // Shrink window from left while product is >= k
            while (product >= k) {
                // Remove leftmost element from product
                product /= nums[left];
                // Move left pointer
                left++;
            }

            // Add count of valid subarrays ending at current right pointer
            // For window of size n, number of subarrays = n
            // Window size = right - left + 1
            count += right - left + 1;
        }

        return count;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Normal case with mixed numbers
        int[] nums1 = {10, 5, 2, 6};
        int k1 = 100;
        System.out.println("Test Case 1 Result: " + numSubarrayProductLessThanK(nums1, k1));
        // Expected output: 8
        // Valid subarrays: [10], [5], [2], [6], [10,5], [5,2], [2,6], [5,2,6]

        // Test Case 2: Edge case with k = 0
        int[] nums2 = {1, 2, 3};
        int k2 = 0;
        System.out.println("Test Case 2 Result: " + numSubarrayProductLessThanK(nums2, k2));
        // Expected output: 0
        // No valid subarrays as all products are positive

        // Test Case 3: Special case with all ones
        int[] nums3 = {1, 1, 1};
        int k3 = 2;
        System.out.println("Test Case 3 Result: " + numSubarrayProductLessThanK(nums3, k3));
        // Expected output: 6
        // All possible subarrays are valid as their products are 1
    }
}

```





