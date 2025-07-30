---
toc:
beginning: true
giscus_comments: true
layout: post
title: "2419. Longest Subarray With Maximum Bitwise AND"
date: "2025-07-30"
tags: Medium
categories:
    - "LeetCode Array"
---


- You are given an integer array `nums` of size `n`.
- Consider a **non-empty** subarray from `nums` that has the **maximum** possible **bitwise AND**.
    - In other words, let `k` be the maximum value of the bitwise AND of **any** subarray of `nums`. Then, only subarrays with a bitwise AND equal to `k` should be considered.
- Return *the length of the **longest** such subarray*.
- The bitwise AND of an array is the bitwise AND of all the numbers in it.
- A **subarray** is a contiguous sequence of elements within an array.

**Example 1**

```
Input: nums = [1,2,3,3,2,2]
Output: 2
Explanation:
The maximum possible bitwise AND of a subarray is 3.
The longest subarray with that value is [3,3], so we return 2.
```

**Example 2**

```
Input: nums = [1,2,3,4]
Output: 1
Explanation:
The maximum possible bitwise AND of a subarray is 4.
The longest subarray with that value is [4], so we return 1.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @Author zhengxingxing
 * @Date 2025/07/30
 */
public class LongestSubarrayWithMaximumBitwiseAND {

    public static int longestSubarray(int[] nums) {
        // Step 1: Find the maximum value in the array.
        int maxVal = nums[0];
        for (int num : nums) {
            if (num > maxVal) {
                maxVal = num;
            }
        }

        // Step 2: Find the longest sequence of consecutive elements equal to maxVal.
        int maxLen = 0;   // To store the maximum length found.
        int currLen = 0;  // To store the current length of consecutive maxVal elements.

        for (int num : nums) {
            if (num == maxVal) {
                // If the current element equals maxVal, increment the current length.
                currLen++;
                // Update maxLen if the current sequence is longer.
                if (currLen > maxLen) {
                    maxLen = currLen;
                }
            } else {
                // If the current element is not maxVal, reset the current length.
                currLen = 0;
            }
        }
        // Return the length of the longest subarray found.
        return maxLen;
    }

    public static void main(String[] args) {
        // Example 1: The longest subarray with maximum bitwise AND is [3, 3], length is 2.
        int[] nums1 = {1, 2, 3, 3, 2, 2};
        // Example 2: The longest subarray with maximum bitwise AND is [4], length is 1.
        int[] nums2 = {1, 2, 3, 4};

        System.out.println(longestSubarray(nums1)); // Output: 2
        System.out.println(longestSubarray(nums2)); // Output: 1
    }
}

```





