---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "795. Number of Subarrays with Bounded Maximum"
date: "2025-04-06"
tags: Medium TwoPointers
categories:
  - "LeetCode ThreePointers"
---


- Given an integer array `nums` and two integers `left` and `right`, return *the number of contiguous non-empty **subarrays** such that the value of the maximum array element in that subarray is in the range* `[left, right]`.
- The test cases are generated so that the answer will fit in a **32-bit** integer.

**Example 1**

```
Input: nums = [2,1,4,3], left = 2, right = 3
Output: 3
Explanation: There are three subarrays that meet the requirements: [2], [2, 1], [3].
```

**Example 2**

```
Input: nums = [2,9,2,5,6], left = 2, right = 8
Output: 7
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
public class NumberOfSubarraysWithBoundedMaximum {

    public static int numSubarrayBoundedMax(int[] nums, int left, int right) {
        int n = nums.length;
        int ans = 0;  // stores final count of valid subarrays
        int lastInvalidPos = -1;  // position of last element > right
        int lastValid = -1;  // position of last element >= left

        // Iterate through array once
        for (int i = 0; i < n; i++) {
            // If current element exceeds right boundary
            if (nums[i] > right){
                lastInvalidPos = i;
            }

            // If current element is within or above left boundary
            if (nums[i] >= left){
                lastValid = i;
            }

            // Add count of valid subarrays ending at current position
            ans += lastValid - lastInvalidPos;
        }

        return ans;
    }


    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {2, 1, 4, 3};
        int left1 = 2;
        int right1 = 3;
        System.out.println("Test Case 1 Result: " +
                numSubarrayBoundedMax(nums1, left1, right1)); // Expected output: 3

        // Test case 2
        int[] nums2 = {2, 9, 2, 5, 6};
        int left2 = 2;
        int right2 = 8;
        System.out.println("Test Case 2 Result: " +
                numSubarrayBoundedMax(nums2, left2, right2)); // Expected output: 7
    }
}

```





