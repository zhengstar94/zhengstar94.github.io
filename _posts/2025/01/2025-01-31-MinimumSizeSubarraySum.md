---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "209. Minimum Size Subarray Sum"
date: "2025-01-31"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindowMin"
---


- Given an array of positive integers `nums` and a positive integer `target`, return *the **minimal length** of a* *subarray* *whose sum is greater than or equal to* `target`. If there is no such subarray, return `0` instead.


**Example 1**

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

**Example 2**

```
Input: target = 4, nums = [1,4,4]
Output: 1
```

**Example 3**

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowMin;

/**
 * @author zhengxingxing
 * @date 2025/01/31
 */
public class MinimumSizeSubarraySum {
    public static int minSubArrayLen(int target, int[] nums) {
        int result = Integer.MAX_VALUE;
        int sum = 0;  // sum of subarray
        int start = 0;  // start position of sliding window

        for (int end = 0; end < nums.length; end++) {
            sum += nums[end];
            // try to shrink the window when sum is greater than or equal to target
            while (sum >= target) {
                result = Math.min(result, end - start + 1);
                sum -= nums[start];
                start++;
            }
        }

        return result == Integer.MAX_VALUE ? 0 : result;
    }

    public static void main(String[] args) {
        // test case 1
        int[] nums1 = {2,3,1,2,4,3};
        int target1 = 7;
        System.out.println("Test case 1 result: " + minSubArrayLen(target1, nums1)); // Expected output: 2

        // test case 2
        int[] nums2 = {1,4,4};
        int target2 = 4;
        System.out.println("Test case 2 result: " + minSubArrayLen(target2, nums2)); // Expected output: 1

        // test case 3
        int[] nums3 = {1,1,1,1,1,1,1,1};
        int target3 = 11;
        System.out.println("Test case 3 result: " + minSubArrayLen(target3, nums3)); // Expected output: 0
    }
}

```





