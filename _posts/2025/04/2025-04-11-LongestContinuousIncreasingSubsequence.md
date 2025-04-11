---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "674. Longest Continuous Increasing Subsequence"
date: "2025-04-11"
tags: Easy
categories:
  - "LeetCode GroupedLoop"
---


- Given an unsorted array of integers `nums`, return *the length of the longest **continuous increasing subsequence** (i.e. subarray)*. The subsequence must be **strictly** increasing.
- A **continuous increasing subsequence** is defined by two indices `l` and `r` (`l < r`) such that it is `[nums[l], nums[l + 1], ..., nums[r - 1], nums[r]]` and for each `l <= i < r`, `nums[i] < nums[i + 1]`.

**Example 1**

```
Input: nums = [1,3,5,4,7]
Output: 3
Explanation: The longest continuous increasing subsequence is [1,3,5] with length 3.
Even though [1,3,5,7] is an increasing subsequence, it is not continuous as elements 5 and 7 are separated by element
4.
```

**Example 2**

```
Input: nums = [2,2,2,2,2]
Output: 1
Explanation: The longest continuous increasing subsequence is [2] with length 1. Note that it must be strictly
increasing.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/11
 */
public class LongestContinuousIncreasingSubsequence {

    public static int findLengthOfLCIS(int[] nums) {
        // Handle edge cases: null or empty array
        if (nums == null || nums.length == 0){
            return 0;
        }

        // Handle single element array
        if (nums.length == 1){
            return 1;
        }

        // Initialize tracking variables
        int maxLength = 0;    // Tracks maximum length found
        int i = 0;           // Current position in array
        int n = nums.length; // Array length

        // Process array using grouped loop pattern
        while (i < n - 1){
            // Mark start of current increasing sequence
            int start = i;

            // Find end of current increasing sequence
            // Continue while next element is larger than current
            while (i < n - 1 && nums[i] < nums[i + 1]){
                i++;
            }

            // Calculate length of current increasing sequence
            int currentLength = i - start + 1;
            // Update maximum length if current sequence is longer
            maxLength = Math.max(maxLength, currentLength);
            // Move to next potential sequence
            i++;
        }

        return maxLength;
    }

    public static void main(String[] args) {
        // Test Case 1: Mixed sequence with partial increase
        System.out.println("Test Case 1:");
        int[] nums1 = {1, 3, 5, 4, 7};
        System.out.println("Input: nums = " + arrayToString(nums1));
        System.out.println("Output: " + findLengthOfLCIS(nums1));

        // Test Case 2: Sequence with all equal elements
        System.out.println("\nTest Case 2:");
        int[] nums2 = {2, 2, 2, 2, 2};
        System.out.println("Input: nums = " + arrayToString(nums2));
        System.out.println("Output: " + findLengthOfLCIS(nums2));

        // Test Case 3: Strictly increasing sequence
        System.out.println("\nTest Case 3:");
        int[] nums3 = {1, 2, 3, 4, 5};
        System.out.println("Input: nums = " + arrayToString(nums3));
        System.out.println("Output: " + findLengthOfLCIS(nums3));
    }

    private static String arrayToString(int[] nums) {
        StringBuilder sb = new StringBuilder("[");
        for (int i = 0; i < nums.length; i++) {
            sb.append(nums[i]);
            if (i < nums.length - 1) {
                sb.append(",");
            }
        }
        sb.append("]");
        return sb.toString();
    }
}

```





