---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2461. Maximum Sum of Distinct Subarrays With Length K"
date: "2024-12-27"
tags: Medium SlideWindow
categories:
  - "LeetCode SlideWindow"
---

- You are given an integer array `nums` and an integer `k`. Find the maximum subarray sum of all the subarrays of `nums` that meet the following conditions:
  - The length of the subarray is `k`, and
  - All the elements of the subarray are **distinct**.
- Return *the maximum subarray sum of all the subarrays that meet the conditions**.* If no subarray meets the conditions, return `0`.
- *A **subarray** is a contiguous non-empty sequence of elements within an array.*

**Example 1**

```
Input: nums = [1,5,4,2,9,9,9], k = 3
Output: 15
Explanation: The subarrays of nums with length 3 are:
- [1,5,4] which meets the requirements and has a sum of 10.
- [5,4,2] which meets the requirements and has a sum of 11.
- [4,2,9] which meets the requirements and has a sum of 15.
- [2,9,9] which does not meet the requirements because the element 9 is repeated.
- [9,9,9] which does not meet the requirements because the element 9 is repeated.
We return 15 because it is the maximum subarray sum of all the subarrays that meet the conditions
```

**Example 2**

```
Input: nums = [4,4,4], k = 3
Output: 0
Explanation: The subarrays of nums with length 3 are:
- [4,4,4] which does not meet the requirements because the element 4 is repeated.
We return 0 because no subarrays meet the conditions.
```

## Method 1

```tex
【O(n) time | O(k) space】
```

```java
package Leetcode.SlideWindow;

import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2024/12/27
 */
public class MaximumSumOfDistinctSubarraysWithLengthK {
    public static long maximumSubarraySum(int[] nums, int k) {
        int n = nums.length;
        if(n == 0){
            return 0;
        }

        // Map to store frequency of elements in current window
        Map<Integer, Integer> count = new HashMap<>();
        // Sum of elements in current window
        long windowSum = 0;
        // Maximum sum found so far
        long maxSum = 0;

        for (int i = 0; i < n; i++) {
            // Add current element to window and update its frequency in map
            count.put(nums[i], count.getOrDefault(nums[i], 0 ) + 1);
            windowSum += nums[i];

            // Skip until we have a complete window of size k
            if(i < k - 1){
                continue;
            }

            // If number of distinct elements equals k, update maximum sum
            if (count.size() == k){
                maxSum = Math.max(maxSum, windowSum);
            }

            // Remove leftmost element from window
            int out = nums[i - k + 1];
            // Decrease its frequency in map
            count.put(out, count.get(out) - 1);
            // If frequency becomes 0, remove element from map to maintain correct distinct count
            if (count.get(out) == 0){
                count.remove(out);
            }
            // Subtract the removed element from window sum
            windowSum -= out;
        }

        return maxSum;
    }

    public static void main(String[] args) {
        // Test Case 1: Contains distinct and repeated elements
        int[] nums1 = {1, 5, 4, 2, 9, 9, 9};
        int k1 = 3;
        System.out.println("Test Case 1 Output: " + maximumSubarraySum(nums1, k1)); // Expected output: 15

        // Test Case 2: All elements are same
        int[] nums2 = {4, 4, 4};
        int k2 = 3;
        System.out.println("Test Case 2 Output: " + maximumSubarraySum(nums2, k2)); // Expected output: 0

        // Test Case 3: All elements are distinct
        int[] nums3 = {1, 2, 3, 4, 5};
        int k3 = 3;
        System.out.println("Test Case 3 Output: " + maximumSubarraySum(nums3, k3)); // Expected output: 12
    }
}

```





