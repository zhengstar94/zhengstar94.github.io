---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1838. Frequency of the Most Frequent Element"
date: "2025-01-21"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindow"
---

- The **frequency** of an element is the number of times it occurs in an array.
- You are given an integer array `nums` and an integer `k`. In one operation, you can choose an index of `nums` and increment the element at that index by `1`.
- Return *the **maximum possible frequency** of an element after performing **at most*** `k` *operations*.

**Example 1**

```
Input: nums = [1,2,4], k = 5
Output: 3
Explanation: Increment the first element three times and the second element two times to make nums = [4,4,4].
4 has a frequency of 3.
```

**Example 2**

```
Input: nums = [1,4,8,13], k = 5
Output: 2
Explanation: There are multiple optimal solutions:
- Increment the first element three times to make nums = [4,4,8,13]. 4 has a frequency of 2.
- Increment the second element four times to make nums = [1,8,8,13]. 8 has a frequency of 2.
- Increment the third element five times to make nums = [1,4,13,13]. 13 has a frequency of 2.
```

**Example 3**

```
Input: nums = [3,9,6], k = 2
Output: 1
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/01/21
 */
public class FrequencyOfTheMostFrequentElement {
    public static int maxFrequency(int[] nums, int k) {
        // Sort array to enable sliding window approach
        // After sorting, we ensure that elements in any window can only be increased to match the rightmost element
        Arrays.sort(nums);

        // Initialize sliding window pointers and variables
        int left = 0;                  // Left boundary of sliding window
        int windowSum = 0;             // Sum of all elements in current window
        int result = 1;                // Minimum possible result is 1 (single element)

        // Iterate through array with right pointer
        for (int right = 0; right < nums.length; right++) {
            // Add current element to window sum
            windowSum += nums[right];

            /**
             * Key Part: Window Validity Check
             * Formula: windowSum + k < nums[right] * (right - left + 1)
             *
             * Left side (windowSum + k):
             * - windowSum: current sum of all elements in window
             * - k: available operations we can use
             * - Together they represent the maximum sum we can achieve with given operations
             *
             * Right side (nums[right] * (right - left + 1)):
             * - nums[right]: target value (maximum element in current window)
             * - (right - left + 1): window size
             * - Product represents total sum needed to make all elements equal to nums[right]
             *
             * If left side < right side: window is invalid and needs shrinking
             */
            while (windowSum + k < (long)nums[right] * (right - left + 1)) {
                // Shrink window by removing leftmost element
                windowSum -= nums[left];
                // Move left pointer to shrink window
                left++;
            }

            // Update result with current valid window size
            // Window size represents the frequency we can achieve
            result = Math.max(result, right - left + 1);
        }

        return result;
    }

    public static void main(String[] args) {
        // Test Case 1: Basic case with small array
        // We can increment 1 three times and 2 two times to get [4,4,4]
        int[] nums1 = {1, 2, 4};
        int k1 = 5;
        System.out.println("Test Case 1 Result: " + maxFrequency(nums1, k1)); // Expected: 3

        // Test Case 2: Multiple optimal solutions possible
        // Can either make two 4s, two 8s, or two 13s
        int[] nums2 = {1, 4, 8, 13};
        int k2 = 5;
        System.out.println("Test Case 2 Result: " + maxFrequency(nums2, k2)); // Expected: 2

        // Test Case 3: Limited operations available
        // Can't make more than one element equal with only 2 operations
        int[] nums3 = {3, 9, 6};
        int k3 = 2;
        System.out.println("Test Case 3 Result: " + maxFrequency(nums3, k3)); // Expected: 1

        // Test Case 4: Array with duplicates
        // Tests handling of already existing frequencies
        int[] nums4 = {1, 1, 1, 2, 2, 4};
        int k4 = 2;
        System.out.println("Test Case 4 Result: " + maxFrequency(nums4, k4)); // Expected: 4
    }
}

```





