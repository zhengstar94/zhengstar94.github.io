---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2444. Count Subarrays With Fixed Bounds"
date: "2025-04-06"
tags: Medium TwoPointers
categories:
  - "LeetCode ThreePointers"
---


- You are given an integer array `nums` and two integers `minK` and `maxK`.
- A **fixed-bound subarray** of `nums` is a subarray that satisfies the following conditions:
  - Return *the **number** of fixed-bound subarrays*.
  - A **subarray** is a **contiguous** part of an array.
- Return *the **number** of fixed-bound subarrays*.
- A **subarray** is a **contiguous** part of an array.

**Example 1**

```
Input: nums = [1,3,5,2,7,5], minK = 1, maxK = 5
Output: 2
Explanation: The fixed-bound subarrays are [1,3,5] and [1,3,5,2].
```

**Example 2**

```
Input: nums = [1,1,1,1], minK = 1, maxK = 1
Output: 10
Explanation: Every subarray of nums is a fixed-bound subarray. There are 10 possible subarrays.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer.ThreePointer;

/**
 * @author zhengxingxing
 * @date 2025/04/06
 */
public class CountSubarraysWithFixedBounds {
    public static long countSubarrays(int[] nums, int minK, int maxK) {
        // Store the final count of valid subarrays
        long result = 0;

        // lastOutOfBound: Index of the last element that was out of bounds [minK, maxK]
        // Initially -1 indicating no out-of-bounds element found yet
        int lastOutOfBound = -1;

        // lastMin: Index of the last occurrence of minK
        // lastMax: Index of the last occurrence of maxK
        // Initially -1 indicating no valid minK or maxK found yet
        int lastMin = -1;
        int lastMax = -1;

        // Iterate through each element in the array
        for (int i = 0; i < nums.length; i++) {
            // Check if current element is within bounds [minK, maxK]
            if (nums[i] >= minK && nums[i] <= maxK) {
                // Update lastMin if we find minK
                if (nums[i] == minK) {
                    lastMin = i;
                }

                // Update lastMax if we find maxK
                if (nums[i] == maxK) {
                    lastMax = i;
                }

                // If we have found both minK and maxK
                if (lastMin != -1 && lastMax != -1) {
                    // Calculate new valid subarrays:
                    // Take the minimum of lastMin and lastMax (earliest position we must include)
                    // Subtract lastOutOfBound (position we can't include)
                    // Use Math.max to ensure we don't add negative values
                    result += Math.max(0, Math.min(lastMin, lastMax) - lastOutOfBound);
                }
            } else {
                // If current element is out of bounds:
                // 1. Update lastOutOfBound to current position
                // 2. Reset lastMin and lastMax as we need to find new occurrences
                lastOutOfBound = i;
                lastMin = -1;
                lastMax = -1;
            }
        }
        return result;
    }

    public static void main(String[] args) {
        // Test Case 1: Array with multiple valid subarrays
        int[] nums1 = {1, 3, 5, 2, 7, 5};
        int minK1 = 1;
        int maxK1 = 5;
        System.out.println("Test Case 1 Result: " + countSubarrays(nums1, minK1, maxK1)); // Expected: 2
        // Valid subarrays: [1,3,5], [1,3,5,2]

        // Test Case 2: Array with all elements equal
        int[] nums2 = {1, 1, 1, 1};
        int minK2 = 1;
        int maxK2 = 1;
        System.out.println("Test Case 2 Result: " + countSubarrays(nums2, minK2, maxK2)); // Expected: 10
        // All possible subarrays are valid
    }
}

```





