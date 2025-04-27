---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3392. Count Subarrays of Length Three With a Condition"
date: "2025-04-27"
tags: Easy
categories:
  - "LeetCode SlideWindow"
---


- Given an integer array `nums`, return the number of subarrays of length 3 such that the sum of the first and third numbers equals *exactly* half of the second number.

**Example 1**

```
Input: nums = [1,2,1,4,1]

Output: 1

Explanation:

Only the subarray [1,4,1] contains exactly 3 elements where the sum of the first and third numbers equals half the middle number.
```

**Example 2**

```
Input: nums = [1,1,1]

Output: 0

Explanation:

[1,1,1] is the only subarray of length 3. However, its first and third numbers do not add to half the middle number.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2025/04/27
 */
public class CountSubarraysOfLengthThreeWithACondition {

    public static int countSubArrays(int[] nums) {
        // Return 0 if array length is less than 3 as we need at least 3 elements
        if (nums.length < 3) {
            return 0;
        }

        int count = 0;

        // Use sliding window of size 3 to check each subarray
        // Loop runs from first element to last possible position for a size-3 window
        for (int i = 0; i <= nums.length - 3; i++) {
            // Check if sum of first and third number equals half of middle number
            // nums[i]: first number
            // nums[i + 1]: middle number
            // nums[i + 2]: third number
            if (nums[i] + nums[i + 2] == nums[i + 1] * 0.5) {
                count++;
            }
        }

        return count;
    }


    public static void main(String[] args) {
        // Test Case 1: Array with valid subarray
        // Expected output: 1 (subarray [1,4,1] satisfies the condition)
        int[] nums1 = {1, 2, 1, 4, 1};
        System.out.println("Test Case 1 Result: " + countSubArrays(nums1));

        // Test Case 2: Array with no valid subarray
        // Expected output: 0 (no subarray satisfies the condition)
        int[] nums2 = {1, 1, 1};
        System.out.println("Test Case 2 Result: " + countSubArrays(nums2));

        // Test Case 3: Empty array
        // Expected output: 0 (array length less than 3)
        int[] nums3 = {};
        System.out.println("Test Case 3 Result: " + countSubArrays(nums3));

        // Test Case 4: Array with length less than 3
        // Expected output: 0 (array length less than 3)
        int[] nums4 = {1, 2};
        System.out.println("Test Case 4 Result: " + countSubArrays(nums4));
    }
}

```





