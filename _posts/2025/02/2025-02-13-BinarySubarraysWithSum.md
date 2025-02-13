---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "930. Binary Subarrays With Sum"
date: "2025-02-13"
tags: Medium SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowCountExact"
---

- Given a binary array `nums` and an integer `goal`, return *the number of non-empty **subarrays** with a sum* `goal`.
- A **subarray** is a contiguous part of the array.


**Example 1**

```
Input: nums = [1,0,1,0,1], goal = 2
Output: 4
Explanation: The 4 subarrays are bolded and underlined below:
[1,0,1,0,1]
[1,0,1,0,1]
[1,0,1,0,1]
[1,0,1,0,1]
```

**Example 2**

```
Input: nums = [0,0,0,0,0], goal = 0
Output: 15
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowCountExact;

/**
 * @author zhengxingxing
 * @date 2025/02/13
 */
public class BinarySubarraysWithSum {
    
    public static int numSubarraysWithSum(int[] nums, int goal) {
        // Result counter to store the number of valid subarrays
        int res = 0;

        // sum1: tracks sum for window1 (sum >= goal)
        // sum2: tracks sum for window2 (sum >= goal + 1)
        int sum1 = 0;
        int sum2 = 0;

        // Using three pointers:
        // l1: left boundary for sum >= goal
        // l2: left boundary for sum >= goal + 1
        // r: right boundary for both windows
        for (int l1 = 0, l2 = 0, r = 0; r < nums.length; r++) {
            // Expand both windows by adding the current element
            sum1 += nums[r];
            sum2 += nums[r];

            // First window: contract from left while sum1 >= goal
            // This window finds all subarrays with sum >= goal
            while (l1 <= r && sum1 >= goal) {
                sum1 -= nums[l1];
                l1++;
            }

            // Second window: contract from left while sum2 >= goal + 1
            // This window finds all subarrays with sum >= goal + 1
            // The purpose is to exclude subarrays with sum greater than goal
            while (l2 <= r && sum2 >= goal + 1) {
                sum2 -= nums[l2];
                l2++;
            }

            // The difference (l1 - l2) gives us the count of subarrays
            // with sum exactly equal to goal ending at current right pointer 'r'
            // Because:
            // l1 represents: how many positions we can place left boundary to get sum >= goal
            // l2 represents: how many positions we can place left boundary to get sum >= goal+1
            // Their difference is exactly the count of subarrays with sum == goal
            res += l1 - l2;
        }

        return res;
    }

    public static void main(String[] args) {
        // Test Case 1: Regular case with mixed 0s and 1s
        // Expected subarrays with sum=2: [1,0,1], [1,0,1], [0,1,0,1], [1,0,1]
        int[] nums1 = {1, 0, 1, 0, 1};
        int goal1 = 2;
        System.out.println("Test Case 1 Result: " + numSubarraysWithSum(nums1, goal1)); // Output: 4

        // Test Case 2: Array with all zeros
        // Every subarray has sum=0, total number of subarrays = n*(n+1)/2 where n=5
        int[] nums2 = {0, 0, 0, 0, 0};
        int goal2 = 0;
        System.out.println("Test Case 2 Result: " + numSubarraysWithSum(nums2, goal2)); // Output: 15

        // Test Case 3: Array with all ones
        // Subarrays with sum=3: [1,1,1], [1,1,1], [1,1,1]
        int[] nums3 = {1, 1, 1, 1, 1};
        int goal3 = 3;
        System.out.println("Test Case 3 Result: " + numSubarraysWithSum(nums3, goal3)); // Output: 3
    }
}

```





