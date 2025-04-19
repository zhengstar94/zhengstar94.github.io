---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2765. Longest Alternating Subarray"
date: "2025-04-19"
tags: Easy
categories:
  - "LeetCode GroupedLoop"
---


- You are given a **0-indexed** integer array `nums`. A subarray `s` of length `m` is called **alternating** if:
  - `m` is greater than `1`.
  - `s1 = s0 + 1`.
  - The **0-indexed** subarray `s` looks like `[s0, s1, s0, s1,...,s(m-1) % 2]`. In other words, `s1 - s0 = 1`, `s2 - s1 = -1`, `s3 - s2 = 1`, `s4 - s3 = -1`, and so on up to `s[m - 1] - s[m - 2] = (-1)m`.
- Return *the maximum length of all **alternating** subarrays present in* `nums` *or* `-1` *if no such subarray exists**.*
- A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1**

```
Input: nums = [2,3,4,3,4]

Output: 4

Explanation:

The alternating subarrays are [2, 3], [3,4], [3,4,3], and [3,4,3,4]. The longest of these is [3,4,3,4], which is of length 4.
```

**Example 2**

```
Input: nums = [4,5,6]

Output: 2

Explanation:

[4,5] and [5,6] are the only two alternating subarrays. They are both of length 2.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/19
 */
public class LongestAlternatingSubarray {

    public static int alternatingSubarray(int[] nums) {
        int n = nums.length;
        // An alternating subarray needs at least 2 elements
        if (n < 2) {
            return -1;
        }

        // Track the maximum length of any valid alternating subarray found
        int maxLen = -1;
        // Current position in the array
        int i = 0;

        // Outer loop: Find potential starting points for alternating subarrays
        while (i < n - 1) {
            // Check if current pair can start an alternating sequence
            // The first two numbers must differ by exactly 1 (increasing)
            if (nums[i + 1] - nums[i] != 1) {
                i++;
                continue;
            }

            // Mark the start of a potential alternating sequence
            int start = i;
            i++;

            // Inner loop: Extend the current alternating sequence as far as possible
            while (i < n - 1) {
                // Calculate the distance from the start of current sequence
                // This distance determines whether we expect an increase or decrease
                int distance = i - start;

                // Determine if the next number should be larger or smaller
                // If distance is even (0,2,4...), expect increase (+1)
                // If distance is odd (1,3,5...), expect decrease (-1)
                // Example for [3,4,3,4]:
                // distance=0: 3->4 (+1)
                // distance=1: 4->3 (-1)
                // distance=2: 3->4 (+1)
                int expectedDiff = (distance % 2 == 0) ? 1 : -1;

                // Calculate what the next number should be based on the pattern
                int expectedNext = nums[i] + expectedDiff;

                // If the next number doesn't match our expectation,
                // we've reached the end of this alternating sequence
                if (nums[i + 1] != expectedNext) {
                    break;
                }

                i++;
            }

            // After the sequence ends, update the maximum length if necessary
            // Add 1 to (i - start) because i points to the last valid position
            // Example: If start=1 and i=4, length is 4-1+1=4 elements
            maxLen = Math.max(maxLen, i - start + 1);
        }

        return maxLen;
    }

    /**
     * Main method with test cases demonstrating various scenarios
     * Each test case shows different aspects of alternating subarrays
     */
    public static void main(String[] args) {
        // Test Case 1: Contains the alternating sequence [3,4,3,4]
        int[] nums1 = {2, 3, 4, 3, 4};
        System.out.println("Test case 1: " + alternatingSubarray(nums1));  // Expected: 4

        // Test Case 2: Contains two alternating sequences of length 2
        int[] nums2 = {4, 5, 6};
        System.out.println("Test case 2: " + alternatingSubarray(nums2));  // Expected: 2

        // Test Case 3: No valid alternating sequence (no consecutive +1)
        int[] nums3 = {1, 3, 5};
        System.out.println("Test case 3: " + alternatingSubarray(nums3));  // Expected: -1

        // Test Case 4: Multiple alternating sequences, returns longest
        int[] nums4 = {1, 2, 1, 2, 3, 4, 3};
        System.out.println("Test case 4: " + alternatingSubarray(nums4));  // Expected: 4
    }
}

```





