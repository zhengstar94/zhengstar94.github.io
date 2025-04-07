---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2760. Longest Even Odd Subarray With Threshold"
date: "2025-04-07"
tags: Easy
categories:
  - "LeetCode GroupedLoop"
---


- You are given a **0-indexed** integer array `nums` and an integer `threshold`.
- Find the length of the **longest subarray** of `nums` starting at index `l` and ending at index `r` `(0 <= l <= r < nums.length)` that satisfies the following conditions:
  - `nums[l] % 2 == 0`
  - For all indices `i` in the range `[l, r - 1]`, `nums[i] % 2 != nums[i + 1] % 2`
  - For all indices `i` in the range `[l, r]`, `nums[i] <= threshold`
- Return *an integer denoting the length of the longest such subarray.**
- ***Note:** A **subarray** is a contiguous non-empty sequence of elements within an array.

**Example 1**

```
Input: nums = [3,2,5,4], threshold = 5
Output: 3
Explanation: In this example, we can select the subarray that starts at l = 1 and ends at r = 3 => [2,5,4]. This subarray satisfies the conditions.
Hence, the answer is the length of the subarray, 3. We can show that 3 is the maximum possible achievable length.
```

**Example 2**

```
Input: nums = [1,2], threshold = 2
Output: 1
Explanation: In this example, we can select the subarray that starts at l = 1 and ends at r = 1 => [2]. 
It satisfies all the conditions and we can show that 1 is the maximum possible achievable length.
```

**Example 3**

```
Input: nums = [2,3,4,5], threshold = 4
Output: 3
Explanation: In this example, we can select the subarray that starts at l = 0 and ends at r = 2 => [2,3,4]. 
It satisfies all the conditions.
Hence, the answer is the length of the subarray, 3. We can show that 3 is the maximum possible achievable length.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/07
 */
public class LongestEvenOddSubarrayWithThreshold {

    public static int longestAlternatingSubarray(int[] nums, int threshold) {
        int n = nums.length;    // Length of input array
        int ans = 0;            // Variable to store the maximum length found
        int i = 0;              // Loop counter/pointer

        while (i < n) {
            // Skip numbers that are either above threshold or not even
            // as they cannot be the start of a valid subarray
            if (nums[i] > threshold || nums[i] % 2 != 0) {
                i++;
                continue;
            }

            // Found a valid starting position (even number within threshold)
            int start = i;      // Mark the start of current valid subarray
            i++;                // Move to next position as start is already validated

            // Extend the subarray as long as conditions are met:
            // 1. Within array bounds
            // 2. Current element <= threshold
            // 3. Current element has different parity than previous element
            while (i < n &&
                    nums[i] <= threshold &&
                    nums[i] % 2 != nums[i - 1] % 2) {
                i++;
            }

            // Update the maximum length if current subarray is longer
            // i-start gives the length of current valid subarray
            ans = Math.max(ans, i - start);
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test Case 1: [3,2,5,4], threshold=5
        // Expected output: 3, because subarray [2,5,4] satisfies:
        // - Starts with even number 2
        // - 2 and 5 have different parity, 5 and 4 have different parity
        // - All numbers <= 5
        int[] test1 = {3,2,5,4};
        System.out.println("Test Case 1 Result: " + longestAlternatingSubarray(test1, 5));  // Output: 3

        // Test Case 2: [1,2], threshold=2
        // Expected output: 1, because only [2] satisfies conditions
        int[] test2 = {1,2};
        System.out.println("Test Case 2 Result: " + longestAlternatingSubarray(test2, 2));  // Output: 1

        // Test Case 3: [2,3,4,5], threshold=4
        // Expected output: 3, because subarray [2,3,4] satisfies:
        // - Starts with even number 2
        // - 2 and 3 have different parity, 3 and 4 have different parity
        // - All numbers <= 4
        int[] test3 = {2,3,4,5};
        System.out.println("Test Case 3 Result: " + longestAlternatingSubarray(test3, 4));  // Output: 3

        // Additional Test Cases
        // Test empty array
        int[] test4 = {};
        System.out.println("Empty Array Test Result: " + longestAlternatingSubarray(test4, 5));  // Output: 0

        // Test single even element
        int[] test5 = {2};
        System.out.println("Single Even Element Test Result: " + longestAlternatingSubarray(test5, 5));  // Output: 1

        // Test array with no valid elements
        int[] test6 = {1,3,5,7};
        System.out.println("No Valid Elements Test Result: " + longestAlternatingSubarray(test6, 5));  // Output: 0
    }
}

```





