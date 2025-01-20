---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2239. Find Closest Number to Zero"
date: "2025-01-20"
tags: Easy
categories:
  - "LeetCode Array"
---


- Given an integer array `nums` of size `n`, return *the number with the value **closest** to* `0` *in* `nums`. If there are multiple answers, return *the number with the **largest** value*.

**Example 1**

```
Input: nums = [-4,-2,1,4,8]
Output: 1
Explanation:
The distance from -4 to 0 is |-4| = 4.
The distance from -2 to 0 is |-2| = 2.
The distance from 1 to 0 is |1| = 1.
The distance from 4 to 0 is |4| = 4.
The distance from 8 to 0 is |8| = 8.
Thus, the closest number to 0 in the array is 1.
```

**Example 2**

```
Input: nums = [2,-1,1]
Output: 1
Explanation: 1 and -1 are both the closest numbers to 0, so 1 being larger is returned.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/01/20
 */
public class FindClosestNumberToZero {
    public static int findClosestNumber(int[] nums) {
        int res = 0; // The result closest to zero
        int minDif = Integer.MAX_VALUE; // Initialize minimum difference as positive infinity

        for (int i = 0; i < nums.length; i++) {
            int dif = Math.abs(nums[i]); // Calculate absolute difference from zero
            if (dif < minDif) { // If current difference is smaller
                res = nums[i];
                minDif = dif;
            } else if (dif == minDif && res < 0) { // If differences are equal and current result is negative
                res = nums[i]; // Update to current number (prefer positive over negative)
            }
        }

        return res;
    }

    public static void main(String[] args) {
        // Test case 1: Mixed positive and negative numbers
        int[] nums1 = {-4, -2, 1, 4, 8};
        System.out.println("Test case 1 result: " + findClosestNumber(nums1)); // Expected output: 1

        // Test case 2: Numbers with same absolute value
        int[] nums2 = {2, -1, 1};
        System.out.println("Test case 2 result: " + findClosestNumber(nums2)); // Expected output: 1

        // Edge case: Symmetric positive and negative numbers
        int[] nums3 = {-5, 5};
        System.out.println("Test case 3 result: " + findClosestNumber(nums3)); // Expected output: 5

        // Edge case: Single element array
        int[] nums4 = {0};
        System.out.println("Test case 4 result: " + findClosestNumber(nums4)); // Expected output: 0

        // Edge case: Only positive numbers
        int[] nums5 = {3, 9, 15};
        System.out.println("Test case 5 result: " + findClosestNumber(nums5)); // Expected output: 3

        // Edge case: Only negative numbers
        int[] nums6 = {-7, -3, -12};
        System.out.println("Test case 6 result: " + findClosestNumber(nums6)); // Expected output: -3
    }
}

```





