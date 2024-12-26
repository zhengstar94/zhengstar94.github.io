---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2841. Maximum Sum of Almost Unique Subarray"
date: "2024-12-26"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---


- You are given an integer array `nums` and two positive integers `m` and `k`.
- Return *the **maximum sum** out of all **almost unique** subarrays of length* `k` *of* `nums`. If no such subarray exists, return `0`.
- A subarray of `nums` is **almost unique** if it contains at least `m` distinct elements.
- A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1**

```
Input: nums = [2,6,7,3,1,7], m = 3, k = 4
Output: 18
Explanation: There are 3 almost unique subarrays of size k = 4. These subarrays are [2, 6, 7, 3], [6, 7, 3, 1], and [7, 3, 1, 7]. Among these subarrays, the one with the maximum sum is [2, 6, 7, 3] which has a sum of 18.
```

**Example 2**

```
Input: nums = [5,9,9,2,4,5,4], m = 1, k = 3
Output: 23
Explanation: There are 5 almost unique subarrays of size k. These subarrays are [5, 9, 9], [9, 9, 2], [9, 2, 4], [2, 4, 5], and [4, 5, 4]. Among these subarrays, the one with the maximum sum is [5, 9, 9] which has a sum of 23.
```

**Example 3**

```
Input: nums = [1,2,1,2,1,2,1], m = 3, k = 3
Output: 0
Explanation: There are no subarrays of size k = 3 that contain at least m = 3 distinct elements in the given array [1,2,1,2,1,2,1]. Therefore, no almost unique subarrays exist, and the maximum sum is 0.
```

## Method 1

```tex
【O(n) time | O(k) space】
```

```java
package Leetcode.SlideWindow;

import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2024/12/26
 */
public class MaximumSumOfAlmostUniqueSubarray {
    public static long maxSum(List<Integer> nums, int m, int k) {
        int n = nums.size();
        // Return 0 if array length is less than required window size
        if(n < k){
            return 0;
        }

        // Initialize variables to track maximum sum and current window sum
        long maxSum = 0;
        long currentSum = 0;

        // Map to track frequency of elements in current window
        Map<Integer, Integer> frequency = new HashMap<>();

        // Iterate through the array using sliding window
        for (int i = 0; i < n; i++) {
            // Add current element to window sum and update frequency
            currentSum += nums.get(i);
            frequency.put(nums.get(i), frequency.getOrDefault(nums.get(i), 0 ) + 1);

            // Skip until we have a full window
            if(i < k - 1){
                continue;
            }

            // Update maxSum if current window has required distinct elements
            if (frequency.size() >= m){
                maxSum = Math.max(maxSum, currentSum);
            }

            // Remove leftmost element of window
            int leftElement = nums.get(i - k + 1);
            currentSum -= leftElement;

            // Update frequency map for removed element
            frequency.put(leftElement, frequency.get(leftElement) - 1);
            if (frequency.get(leftElement) == 0){
                frequency.remove(leftElement);
            }
        }

        return maxSum;
    }

    public static void main(String[] args) {
        // Test Case 1: nums = [2,6,7,3,1,7], m = 3, k = 4
        List<Integer> nums1 = Arrays.asList(2, 6, 7, 3, 1, 7);
        int m1 = 3;
        int k1 = 4;
        System.out.println("Test Case 1:");
        System.out.println("Input: nums = " + nums1 + ", m = " + m1 + ", k = " + k1);
        System.out.println("Expected: 18");
        System.out.println("Output: " + maxSum(nums1, m1, k1));
        System.out.println();

        // Test Case 2: nums = [5,9,9,2,4,5,4], m = 1, k = 3
        List<Integer> nums2 = Arrays.asList(5, 9, 9, 2, 4, 5, 4);
        int m2 = 1;
        int k2 = 3;
        System.out.println("Test Case 2:");
        System.out.println("Input: nums = " + nums2 + ", m = " + m2 + ", k = " + k2);
        System.out.println("Expected: 23");
        System.out.println("Output: " + maxSum(nums2, m2, k2));
        System.out.println();

        // Test Case 3: nums = [1,2,1,2,1,2,1], m = 3, k = 3
        List<Integer> nums3 = Arrays.asList(1, 2, 1, 2, 1, 2, 1);
        int m3 = 3;
        int k3 = 3;
        System.out.println("Test Case 3:");
        System.out.println("Input: nums = " + nums3 + ", m = " + m3 + ", k = " + k3);
        System.out.println("Expected: 0");
        System.out.println("Output: " + maxSum(nums3, m3, k3));
    }
}

```





