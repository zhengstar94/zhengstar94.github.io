---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1493. Longest Subarray of 1's After Deleting One Element"
date: "2025-01-12"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindow"
---

- Given a binary array `nums`, you should delete one element from it.
- Return *the size of the longest non-empty subarray containing only* `1`*'s in the resulting array*. Return `0` if there is no such subarray.

**Example 1**

```
Input: nums = [1,1,0,1]
Output: 3
Explanation: After deleting the number in position 2, [1,1,1] contains 3 numbers with value of 1's.
```

**Example 2**

```
Input: nums = [0,1,1,1,0,1,1,0,1]
Output: 5
Explanation: After deleting the number in position 4, [0,1,1,1,1,1,0,1] longest subarray with value of 1's is [1,1,1,1,1].
```

**Example 3**

```
Input: nums = [1,1,1]
Output: 2
Explanation: You must delete one element.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/12
 */
public class LongestSubarrayOf1sAfteDeletingOneElement {
    public static int longestSubarray(int[] nums) {
        int zeroCount = 0; // Keeps track of the number of zeros in the current window
        int maxLength = 0; // Tracks the length of the longest subarray found
        int start = 0;     // Left boundary of the sliding window

        for (int end = 0; end < nums.length; end++) {
            // 1. Enter window: The current element is added to the window
            if (nums[end] == 0) {
                zeroCount++; // Increment zero count if the current element is 0
            }

            // 2. Exit window: Adjust the window to ensure the condition (at most one zero) is satisfied
            while (zeroCount > 1) { // If there are more than one zero in the window
                if (nums[start] == 0) {
                    zeroCount--; // Decrement zero count if the leftmost element is 0
                }
                start++; // Move the left boundary of the window to the right
            }

            // 3. Update answer: Update the maximum length for a valid subarray
            maxLength = Math.max(maxLength, end - start); // Calculate the current valid window size
        }

        // Special case: If the array consists entirely of 1s, the result should exclude one element
        return maxLength == nums.length ? maxLength - 1 : maxLength;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {1, 1, 0, 1};
        System.out.println("Test case 1: " + longestSubarray(nums1)); // Expected output: 3

        // Test case 2
        int[] nums2 = {0, 1, 1, 1, 0, 1, 1, 0, 1};
        System.out.println("Test case 2: " + longestSubarray(nums2)); // Expected output: 5

        // Test case 3
        int[] nums3 = {1, 1, 1};
        System.out.println("Test case 3: " + longestSubarray(nums3)); // Expected output: 2

        // Additional test case 1
        int[] nums4 = {0, 0, 0};
        System.out.println("Test case 4: " + longestSubarray(nums4)); // Expected output: 0

        // Additional test case 2
        int[] nums5 = {1, 0, 1, 0, 1};
        System.out.println("Test case 5: " + longestSubarray(nums5)); // Expected output: 2
    }
}

```





