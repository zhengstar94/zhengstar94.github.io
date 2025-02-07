---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2962. Count Subarrays Where Max Element Appears at Least K Times"
date: "2025-02-03"
tags: Medium SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowCountLongest"
---

- You are given an integer array `nums` and a **positive** integer `k`.
- Return *the number of subarrays where the **maximum** element of* `nums` *appears **at least*** `k` *times in that subarray.*
- A **subarray** is a contiguous sequence of elements within (prep. 在…之内 adv. 在内部) an array.

**Example 1**

```
Input: nums = [1,3,2,3,3], k = 2
Output: 6
Explanation: The subarrays that contain the element 3 at least 2 times are: [1,3,2,3], [1,3,2,3,3], [3,2,3], [3,2,3,3], [2,3,3] and [3,3].
```

**Example 2**

```
Input: nums = [1,4,2,1], k = 3
Output: 0
Explanation: No subarray contains the element 4 at least 3 times.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowCountLongest;

/**
 * @author zhengxingxing
 * @date 2025/02/03
 */
public class CountSubarraysWhereMaxElementAppearsAtLeastKTimes {

    public static long countSubarrays(int[] nums, int k) {
        // 1. Find the maximum value in the array
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }

        // 2. Process using sliding window technique
        long ans = 0;  // Store the final count of valid subarrays
        int cntMx = 0; // Count of maximum value occurrences in current window
        int left = 0;  // Left pointer of the sliding window

        // Iterate through array using right pointer
        for (int x : nums) {
            // If current element is the maximum value, increment the counter
            if (x == mx) {
                cntMx++;
            }

            /**
             * Key Part 1: Window Adjustment
             * When we find exactly k occurrences of maximum value in the window:
             * 1. We need to shrink the window from left until cntMx < k
             * 2. This helps us find the leftmost valid position for the current right pointer
             *
             * For example, in [1,3,2,3,3] with k=2:
             * When right=3 (fourth position), window contains [1,3,2,3]
             * We move left pointer until we have less than k max values
             */
            while (cntMx == k) {
                // If we remove a maximum value from the left, decrease the counter
                if (nums[left++] == mx) {
                    cntMx--;
                }
            }

            /**
             * Key Part 2: Counting Valid Subarrays
             * After the while loop:
             * - 'left' represents the position where window becomes invalid
             * - All positions before 'left' can be valid starting points
             *
             * For example, when right=3 in [1,3,2,3]:
             * If left=2, we can start subarrays from:
             * - position 0: [1,3,2,3]
             * - position 1: [3,2,3]
             * So we add 'left' (2) to answer
             *
             * This is why ans += left works:
             * - It counts all possible valid starting positions
             * - Each starting position forms exactly one valid subarray with current right pointer
             */
            ans += left;
        }

        return ans;
    }


    public static void main(String[] args) {
        // Test Case 1: Array with maximum value appearing multiple times
        int[] nums1 = {1,3,2,3,3};
        System.out.println("Test Case 1 Result: " + countSubarrays(nums1, 2)); // Expected: 6
        // Valid subarrays are: [1,3,2,3], [3,2,3], [1,3,2,3,3], [3,2,3,3], [2,3,3], [3,3]

        // Test Case 2: Array where maximum value appears less than k times
        int[] nums2 = {1,4,2,1};
        System.out.println("Test Case 2 Result: " + countSubarrays(nums2, 3)); // Expected: 0
        // No valid subarrays as maximum value (4) appears less than 3 times
    }
}

```





