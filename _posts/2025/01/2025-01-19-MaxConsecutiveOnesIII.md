---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1004. Max Consecutive Ones III"
date: "2025-01-19"
tags: Medium SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowMax"
---


- Given a binary array `nums` and an integer `k`, return *the maximum number of consecutive* `1`*'s in the array if you can flip at most* `k` `0`'s.

**Example 1**

```
Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6
Explanation: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

**Example 2**

```
Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10
Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/19
 */
public class MaxConsecutiveOnesIII {
    public static int longestOnes(int[] nums, int k) {
        // Initialize left pointer of the window
        int left = 0;
        // Track the maximum length of valid sequence found so far
        int maxLength = 0;
        // Count of zeros in current window
        int zeroCount = 0;

        // Iterate through array with right pointer
        for (int right = 0; right < nums.length; right++) {
            // If current element is 0, increment zero counter
            if (nums[right] == 0) {
                zeroCount++;
            }

            // While window contains more zeros than allowed (k)
            // Shrink window from left until constraint is satisfied
            while (zeroCount > k) {
                // If element at left pointer is 0, decrease zero counter
                if (nums[left] == 0) {
                    zeroCount--;
                }
                // Move left pointer to shrink window
                left++;
            }

            // Update maximum length if current window is larger
            // Window size is (right - left + 1)
            maxLength = Math.max(maxLength, right - left + 1);
        }

        // Return the maximum length found
        return maxLength;
    }

    // 主方法，包含测试用例
    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 0};
        int k1 = 2;
        System.out.println("Test Case 1 Result: " + longestOnes(nums1, k1)); // Expected output: 6

        // Test case 2
        int[] nums2 = {0, 0, 1, 1, 0, 0, 1, 1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1, 1};
        int k2 = 3;
        System.out.println("Test Case 2 Result: " + longestOnes(nums2, k2)); // Expected output: 10
    }
}

```





