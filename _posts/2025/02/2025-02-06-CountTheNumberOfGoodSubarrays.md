---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "(Review)2537. Count the Number of Good Subarrays"
date: "2025-02-06"
tags: Medium Review SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowCountLongest"
---


- Given an integer array `nums` and an integer `k`, return *the number of **good** subarrays of* `nums`.
- A subarray `arr` is **good** if there are **at least** `k` pairs of indices `(i, j)` such that `i < j` and `arr[i] == arr[j]`.
- A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1**

```
Input: nums = [1,1,1,1,1], k = 10
Output: 1
Explanation: The only good subarray is the array nums itself.
```

**Example 2**

```
Input: nums = [3,1,4,3,2,2,4], k = 2
Output: 4
Explanation: There are 4 different good subarrays:
- [3,1,4,3,2,2] that has 2 pairs.
- [3,1,4,3,2,2,4] that has 3 pairs.
- [1,4,3,2,2,4] that has 2 pairs.
- [4,3,2,2,4] that has 2 pairs.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowCountLongest;

import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2025/02/06
 */
public class CountTheNumberOfGoodSubarrays {
    public static long countGood(int[] nums, int k) {
        // HashMap to store the frequency of each number in the current window
        Map<Integer, Integer> count = new HashMap<>();
        long result = 0;  // Store the total count of good subarrays
        long currentPairs = 0;  // Track the number of pairs in current window
        int left = 0;  // Left pointer of sliding window

        // Iterate through array using right pointer
        for (int right = 0; right < nums.length; right++) {
            // When adding a new element at right, it forms pairs with all its previous occurrences
            // For example: if we have [1,1] and add another 1, it forms 2 new pairs
            currentPairs += count.getOrDefault(nums[right], 0);
            count.merge(nums[right], 1, Integer::sum);

            /**
             * Key Part 1: Window Shrinking Condition
             * while (left <= right && currentPairs - count.get(nums[left]) + 1 >= k)
             *
             * This condition checks if we can shrink the window from the left while still maintaining k pairs:
             * - left <= right: ensures left pointer doesn't exceed right pointer
             * - currentPairs - count.get(nums[left]) + 1 >= k: 
             *   - currentPairs: current total pairs in window
             *   - count.get(nums[left]): number of occurrences of leftmost element
             *   - +1: adjustment factor for remaining pairs after removal
             *   If this condition is true, we can safely remove the leftmost element
             *   and still have enough pairs to meet the requirement
             */
            while (left <= right && currentPairs - count.get(nums[left]) + 1 >= k) {
                // Remove the leftmost element and update pairs count
                currentPairs -= count.merge(nums[left], -1, Integer::sum);
                left++;
            }

            /**
             * Key Part 2: Counting Valid Subarrays
             * if (currentPairs >= k)
             * result += left + 1
             *
             * When we find a window with enough pairs (currentPairs >= k):
             * - left + 1 represents the number of valid subarrays ending at 'right'
             * - For example, if left=2 and right=4, we add 3 because:
             *   We can start the subarray from index 0,1,or 2 (total of 3 possibilities)
             *   all ending at index 4, and each of these subarrays is valid
             * - This counts all possible valid subarrays that end at the current right pointer
             */
            if (currentPairs >= k) {
                result += left + 1;
            }
        }

        return result;
    }

    public static void main(String[] args) {
        // Test Case 1: Array with all same elements
        int[] nums1 = {1, 1, 1, 1, 1};
        int k1 = 10;
        System.out.println("Test Case 1 Result: " + countGood(nums1, k1)); // Expected: 1

        // Test Case 2: Array with different elements
        int[] nums2 = {3, 1, 4, 3, 2, 2, 4};
        int k2 = 2;
        System.out.println("Test Case 2 Result: " + countGood(nums2, k2)); // Expected: 4

        // Test Case 3: Small array with same elements
        int[] nums3 = {1, 1, 1};
        int k3 = 2;
        System.out.println("Test Case 3 Result: " + countGood(nums3, k3)); // Expected: 2
    }
}

```





