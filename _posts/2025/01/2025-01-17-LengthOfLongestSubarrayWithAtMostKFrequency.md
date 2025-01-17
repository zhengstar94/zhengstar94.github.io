---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2958. Length of Longest Subarray With at Most K Frequency"
date: "2025-01-17"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindow"
---


- You are given an integer array `nums` and an integer `k`.
- The **frequency** of an element `x` is the number of times it occurs in an array.
- An array is called **good** if the frequency of each element in this array is **less than or equal** to `k`.
- Return *the length of the **longest** **good** subarray of* `nums`*.*
- A **subarray** is a contiguous non-empty sequence of elements within an array.

**Example 1**

```
Input: nums = [1,2,3,1,2,3,1,2], k = 2
Output: 6
Explanation: The longest possible good subarray is [1,2,3,1,2,3] since the values 1, 2, and 3 occur at most twice in this subarray. Note that the subarrays [2,3,1,2,3,1] and [3,1,2,3,1,2] are also good.
It can be shown that there are no good subarrays with length more than 6.
```

**Example 2**

```
Input: nums = [1,2,1,2,1,2,1,2], k = 1
Output: 2
Explanation: The longest possible good subarray is [1,2] since the values 1 and 2 occur at most once in this subarray. Note that the subarray [2,1] is also good.
It can be shown that there are no good subarrays with length more than 2.
```

**Example 3**

```
Input: nums = [5,5,5,5,5,5,5], k = 4
Output: 4
Explanation: The longest possible good subarray is [5,5,5,5] since the value 5 occurs 4 times in this subarray.
It can be shown that there are no good subarrays with length more than 4.
```

## Method 1

```tex
【O(n) time | O(min(m, n)) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2025/01/17
 */
public class LengthOfLongestSubarrayWithAtMostKFrequency {
    public static int maxSubarrayLength(int[] nums, int k) {
        // HashMap to store the frequency of each element in current window
        Map<Integer, Integer> frequency = new HashMap<>();
        // Variable to keep track of maximum valid subarray length
        int maxLength = 0;
        // Left pointer of the sliding window
        int left = 0;

        // Iterate through the array using right pointer
        for (int right = 0; right < nums.length; right++) {
            // Add current element to frequency map and increment its count
            frequency.put(nums[right], frequency.getOrDefault(nums[right], 0) + 1);

            // Shrink window from left while current element's frequency exceeds k
            while (frequency.get(nums[right]) > k) {
                // Decrease frequency of element at left pointer
                frequency.put(nums[left], frequency.get(nums[left]) - 1);
                // Move left pointer to shrink window
                left++;
            }

            // Update maximum length if current window is larger
            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Elements appear at most twice
        int[] nums1 = {1,2,3,1,2,3,1,2};
        int k1 = 2;
        System.out.println("Test Case 1 Result: " + maxSubarrayLength(nums1, k1)); // Expected output: 6

        // Test Case 2: Elements appear at most once
        int[] nums2 = {1,2,1,2,1,2,1,2};
        int k2 = 1;
        System.out.println("Test Case 2 Result: " + maxSubarrayLength(nums2, k2)); // Expected output: 2

        // Test Case 3: Same element repeated with frequency limit 4
        int[] nums3 = {5,5,5,5,5,5,5};
        int k3 = 4;
        System.out.println("Test Case 3 Result: " + maxSubarrayLength(nums3, k3)); // Expected output: 4
    }
}

```





