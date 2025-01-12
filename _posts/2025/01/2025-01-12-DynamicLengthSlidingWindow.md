---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "Dynamic-Length Sliding Window"
date: "2025-01-12"
categories: 
  - "Data Structure"
---

The sliding window is a powerful and efficient algorithmic technique for solving problems involving subarrays or substrings. It is particularly useful for problems requiring dynamic adjustments to window size, moving away from brute-force solutions. With sliding window techniques, the time complexity is reduced from O(n^2) to O(n)by avoiding unnecessary recalculations.

This document provides a detailed explanation of the core concept, step-by-step implementation, and practical applications of dynamic-length sliding windows.

## Core Concept

The sliding window technique helps efficiently compute subarray (or substring) properties by **dynamically adjusting window boundaries (start and end points)**. The general process involves three key steps:

1. **Window Expansion**: Expand the window by moving the right boundary to include more elements while updating required properties dynamically (e.g., frequency, sum, or count).
2. **Condition Validation**: After every expansion, check whether the current window satisfies the required conditions.
3. **Window Contraction**: If the condition is violated, shrink the window from the left while recalculating the required properties until the condition is restored.

By focusing only on valid windows, this methodology avoids redundant calculations, significantly improving efficiency.

## Example Problem

**Problem**:

Given a binary array `nums` containing only `0` and `1`, delete **exactly one element**, and return the length of the longest subarray that consists only of `1`.

**Input**: nums = [1, 1, 0, 1]

**output**: 3

## Dynamic Sliding Window Steps

For this problem, we apply the sliding window pattern as follows:

1. **Enter Window**: Track the count of zeros as we expand the window.
2. **Validate Condition**: Ensure we don't have more than one zero in our window.
3. **Exit Window**: Shrink the window when we exceed our zero limit.

### Example Walkthrough

Using `nums = [1, 1, 0, 1]`:

1. **Initialize**:
    - Window start: `start = 0`
    - Zero counter: `zeroCount = 0`
    - Maximum length: `maxLength = 0`
2. **Process Array**:
    - **At end = 0** (`nums[0] = 1`): Window: `[1]`, zeros = 0, `maxLength = 1`
    - **At end = 1** (`nums[1] = 1`): Window: `[1, 1]`, zeros = 0, `maxLength = 2`
    - **At end = 2** (`nums[2] = 0`): Window: `[1, 1, 0]`, zeros = 1, `maxLength = 3`
    - **At end = 3** (`nums[3] = 1`): Window: `[1, 1, 0, 1]`, zeros = 1, `maxLength = 4`

## Implementation

```java
class Solution {
    public int longestSubarray(int[] nums) {
        int zeroCount = 0; // Count of zeros in current window
        int maxLength = 0; // Length of longest valid subarray
        int start = 0;     // Start of current window

        for (int end = 0; end < nums.length; end++) {
            // 1. Expand window: Add current element
            if (nums[end] == 0) {
                zeroCount++;
            }

            // 2. Contract window: Remove elements when condition violated
            while (zeroCount > 1) {
                if (nums[start] == 0) {
                    zeroCount--;
                }
                start++;
            }

            // 3. Update result: Calculate current valid window length
            maxLength = Math.max(maxLength, end - start);
        }

        // Handle edge case: all ones array
        return maxLength == nums.length ? maxLength - 1 : maxLength;
    }
}

```

### Explanation of Key Steps

1. **Enter Window**: Add the current element to the window and update the zero count.
2. **Exit Window**: When the number of zeros exceeds 1, shrink the window from the left while reducing the zero count.
3. **Update Answer**: Continuously calculate the size of the valid window and keep track of the maximum length.



## Complexity Analysis

1. **Time Complexity**:
    - O(n): Each element is processed at most twice—once when entering the window and once when exiting.
2. **Space Complexity**:
    - O(1): Only a few extra variables are used to track the window's start and end, with no additional data structures.

## Applications and Scenarios

Dynamic sliding windows are highly versatile and applicable across a wide range of problems. Some common scenarios include:

1. **Longest Subarray Problems**:
    - Example: Finding the longest substring with at most K distinct characters.
2. **Frequency or Condition Checks**:
    - Example: Tracking the frequency of a specific element within a window, ensuring it does not exceed a certain limit.
3. **Subarray/Substring Property Calculation**:
    - Example: Dynamically calculating sums, products, or maximum/minimum values within sliding windows.
4. **Pattern Matching**:
    - Example: Detecting valid substrings or subarrays that meet complex pattern requirements.

Mastering the dynamic sliding window technique allows you to efficiently solve various problems with changing constraints, optimizing your algorithmic performance.

## Edge Cases

When utilizing the sliding window technique, it's essential to consider edge cases to avoid errors in extreme scenarios. Key edge cases for this problem include:

1. **All Ones**:
    - Input: `[1, 1, 1]`
    - Output: Must subtract one, resulting in length `len(nums) - 1`.
2. **All Zeros**:
    - Input: `[0, 0, 0]`
    - Output: No valid subarrays exist, resulting in output `0`.
3. **Empty Input**:
    - Input: `[]`
    - Output: Should return `0` as there are no valid subarrays.
4. **Single Element**:
    - Input: `[1]` or `[0]`
    - Output: Results in `0` due to the removal leaving no elements.

## Summary

By mastering the dynamic sliding window approach, you can efficiently solve a wide variety of problems that require handling variable-sized subarrays or substrings. Its flexibility and high efficiency make it a crucial technique for algorithmic problem-solving.



