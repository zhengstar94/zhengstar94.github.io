---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1984. Minimum Difference Between Highest and Lowest of K Scores"
date: "2025-01-09"
tags: Easy
categories:
  - "LeetCode SlideWindow"
---


- You are given a **0-indexed** integer array `nums`, where `nums[i]` represents the score of the `ith` student. You are also given an integer `k`.
- Pick the scores of any `k` students from the array so that the **difference** between the **highest** and the **lowest** of the `k` scores is **minimized**.
- Return *the **minimum** possible difference*.

**Example 1**

```
Input: nums = [90], k = 1
Output: 0
Explanation: There is one way to pick score(s) of one student:
- [90]. The difference between the highest and lowest score is 90 - 90 = 0.
The minimum possible difference is 0.
```

**Example 2**

```
Input: nums = [9,4,1,7], k = 2
Output: 2
Explanation: There are six ways to pick score(s) of two students:
- [9,4,1,7]. The difference between the highest and lowest score is 9 - 4 = 5.
- [9,4,1,7]. The difference between the highest and lowest score is 9 - 1 = 8.
- [9,4,1,7]. The difference between the highest and lowest score is 9 - 7 = 2.
- [9,4,1,7]. The difference between the highest and lowest score is 4 - 1 = 3.
- [9,4,1,7]. The difference between the highest and lowest score is 7 - 4 = 3.
- [9,4,1,7]. The difference between the highest and lowest score is 7 - 1 = 6.
The minimum possible difference is 2.
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/01/09
 */
public class MinimumDifferenceBetweenHighestAndLowestOfKScores {

    public static int minimumDifference(int[] nums, int k) {
        // Handle edge cases:
        // 1. If array is null
        // 2. If array length is less than k
        // 3. If k is 1 or less (difference will always be 0)
        if (nums == null || nums.length < k || k <= 1) {
            return 0;
        }

        // Sort the array to ensure that within any window of size k,
        // the maximum will be at the right end and minimum at the left end
        Arrays.sort(nums);

        // Initialize minDiff with maximum possible integer value
        int minDiff = Integer.MAX_VALUE;

        // Sliding window implementation:
        // i represents the start of each window
        // i + k - 1 represents the end of each window
        // We use <= because we want to include the last possible window
        // For example: if array length is 6 and k is 3,
        // we need to check windows starting at index 0,1,2,3 (6-3=3)
        for (int i = 0; i <= nums.length - k; i++) {
            // Calculate difference between max and min in current window
            // nums[i + k - 1] is the maximum (right end of window)
            // nums[i] is the minimum (left end of window)
            int diff = nums[i + k - 1] - nums[i];
            // Update minDiff if current difference is smaller
            minDiff = Math.min(minDiff, diff);
        }

        return minDiff;
    }

    /**
     * Main method to test the solution with various test cases
     */
    public static void main(String[] args) {
        // Test case 1: Single element array
        int[] nums1 = {90};
        int k1 = 1;
        System.out.println("Test case 1: " + minimumDifference(nums1, k1)); // Expected output: 0

        // Test case 2: Small array with multiple elements
        int[] nums2 = {9,4,1,7};
        int k2 = 2;
        System.out.println("Test case 2: " + minimumDifference(nums2, k2)); // Expected output: 2
    }
}
```





