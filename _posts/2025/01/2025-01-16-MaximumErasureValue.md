---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1695. Maximum Erasure Value"
date: "2025-01-16"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindow"
---



- You are given an array of positive integers `nums` and want to erase a subarray containing **unique elements**. The **score** you get by erasing the subarray is equal to the **sum** of its elements.
- Return *the **maximum score** you can get by erasing **exactly one** subarray.*
- An array `b` is called to be a subarray of `a` if it forms a contiguous subsequence of `a`, that is, if it is equal to `a[l],a[l+1],...,a[r]` for some `(l,r)`.

**Example 1**

```
Input: nums = [4,2,4,5,6]
Output: 17
Explanation: The optimal subarray here is [2,4,5,6].
```

**Example 2**

```
Input: nums = [5,2,1,2,5,2,1,2,5]
Output: 8
Explanation: The optimal subarray here is [5,2,1] or [1,2,5].
```

## Method 1

```tex
【O(n) time | O(k) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

import java.util.HashSet;
import java.util.Set;

/**
 * @author zhengxingxing
 * @date 2025/01/16
 */
public class MaximumErasureValue {
    public static int maximumUniqueSubarray(int[] nums) {
        // HashSet to keep track of unique elements in current window
        Set<Integer> set = new HashSet<>();

        // Variable to store the maximum sum found so far
        int maxSum = 0;

        // Variable to maintain the sum of current window
        int currentSum = 0;

        // Left pointer of sliding window
        int left = 0;

        // Iterate through array using right pointer
        for (int right = 0; right < nums.length; right++) {
            // While current element is already in set, shrink window from left
            while (set.contains(nums[right])) {
                // Remove leftmost element from set
                set.remove(nums[left]);

                // Subtract removed element from current window sum
                currentSum -= nums[left];

                // Move left pointer to shrink window
                left++;
            }

            // Add current element to set
            set.add(nums[right]);

            // Add current element to window sum
            currentSum += nums[right];

            // Update maximum sum if current window sum is larger
            maxSum = Math.max(maxSum, currentSum);
        }

        // Return the maximum sum found
        return maxSum;
    }


    public static void main(String[] args) {
        // Test Case 1: Array with duplicate elements
        int[] nums1 = {4, 2, 4, 5, 6};
        System.out.println("Test Case 1 Result: " + maximumUniqueSubarray(nums1)); // Expected: 17

        // Test Case 2: Array with multiple duplicates
        int[] nums2 = {5, 2, 1, 2, 5, 2, 1, 2, 5};
        System.out.println("Test Case 2 Result: " + maximumUniqueSubarray(nums2)); // Expected: 8

        // Test Case 3: Array with all unique elements
        int[] nums3 = {1, 2, 3, 4, 5};
        System.out.println("Test Case 3 Result: " + maximumUniqueSubarray(nums3)); // Expected: 15
    }
}

```





