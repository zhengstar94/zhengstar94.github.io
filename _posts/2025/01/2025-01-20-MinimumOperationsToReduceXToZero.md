---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1658. Minimum Operations to Reduce X to Zero"
date: "2025-01-20"
tags: Medium SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowMax"
---


- You are given an integer array `nums` and an integer `x`. In one operation, you can either remove the leftmost or the rightmost element from the array `nums` and subtract its value from `x`. Note that this **modifies** the array for future operations.
- Return *the **minimum number** of operations to reduce* `x` *to **exactly*** `0` *if it is possible**, otherwise, return* `-1`.

**Example 1**

```
Input: nums = [1,1,4,2,3], x = 5
Output: 2
Explanation: The optimal solution is to remove the last two elements to reduce x to zero.
```

**Example 2**

```
Input: nums = [5,6,7,8,9], x = 4
Output: -1
```

**Example 3**

```
Input: nums = [3,2,20,1,1,3], x = 10
Output: 5
Explanation: The optimal solution is to remove the last three elements and the first two elements (5 operations in total) to reduce x to zero.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/20
 */
public class MinimumOperationsToReduceXToZero {
    public static int minOperations(int[] nums, int x) {
        // Step 1: Calculate the total sum of the array
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }

        // Step 2: Calculate target sum for the sliding window
        // We want to find a subarray that sums to (total_sum - x)
        int target = sum - x;

        // If target is negative, it's impossible to find a solution
        if (target < 0) {
            return -1;
        }

        // If target is zero, we need to remove all elements
        if (target == 0) {
            return nums.length;
        }

        // Step 3: Initialize sliding window variables
        int left = 0;              // Left pointer of the window
        int currentSum = 0;        // Current sum within the window
        int maxLen = -1;          // Length of longest subarray summing to target

        // Step 4: Implement sliding window technique
        for (int right = 0; right < nums.length; right++) {
            // Add current element to window sum
            currentSum += nums[right];

            // Shrink window while sum exceeds target
            while (currentSum > target && left < right) {
                currentSum -= nums[left];
                left++;
            }

            // If window sum equals target, update maximum length
            if (currentSum == target) {
                maxLen = Math.max(maxLen, right - left + 1);
            }
        }

        // Step 5: Return result
        // If no valid subarray found, return -1
        if (maxLen == -1) {
            return -1;
        }

        // Return number of elements to remove (total length minus length of valid subarray)
        return nums.length - maxLen;
    }

    public static void main(String[] args) {
        // Test Case 1: Normal case
        // Expected output: 2 (remove 2 and 3 from the end)
        int[] nums1 = {1, 1, 4, 2, 3};
        int x1 = 5;
        System.out.println("Test Case 1 Result: " + minOperations(nums1, x1));

        // Test Case 2: Impossible case
        // Expected output: -1 (no combination sums to 4)
        int[] nums2 = {5, 6, 7, 8, 9};
        int x2 = 4;
        System.out.println("Test Case 2 Result: " + minOperations(nums2, x2));

        // Test Case 3: Multiple operations needed
        // Expected output: 5 (remove [3,2] from left and [1,1,3] from right)
        int[] nums3 = {3, 2, 20, 1, 1, 3};
        int x3 = 10;
        System.out.println("Test Case 3 Result: " + minOperations(nums3, x3));

        // Test Case 4: Edge case - Empty array
        // Expected output: -1 (impossible with empty array)
        int[] nums4 = {};
        int x4 = 1;
        System.out.println("Test Case 4 Result: " + minOperations(nums4, x4));

        // Test Case 5: Edge case - Single element
        // Expected output: 1 (remove the single element)
        int[] nums5 = {1};
        int x5 = 1;
        System.out.println("Test Case 5 Result: " + minOperations(nums5, x5));
    }
}

```





