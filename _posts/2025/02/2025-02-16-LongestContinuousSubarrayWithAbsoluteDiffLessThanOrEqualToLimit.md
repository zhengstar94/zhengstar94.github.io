---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "(Review)1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit"
date: "2025-02-16"
tags: Medium SlideWindow Review
categories:
  - "LeetCode DynamicSlidingWindowMax"
---


- Given an array of integers `nums` and an integer `limit`, return the size of the longest **non-empty** subarray such that the absolute difference between any two elements of this subarray is less than or equal to `limit`*.*


**Example 1**

```
Input: nums = [8,2,4,7], limit = 4
Output: 2 
Explanation: All subarrays are: 
[8] with maximum absolute diff |8-8| = 0 <= 4.
[8,2] with maximum absolute diff |8-2| = 6 > 4. 
[8,2,4] with maximum absolute diff |8-2| = 6 > 4.
[8,2,4,7] with maximum absolute diff |8-2| = 6 > 4.
[2] with maximum absolute diff |2-2| = 0 <= 4.
[2,4] with maximum absolute diff |2-4| = 2 <= 4.
[2,4,7] with maximum absolute diff |2-7| = 5 > 4.
[4] with maximum absolute diff |4-4| = 0 <= 4.
[4,7] with maximum absolute diff |4-7| = 3 <= 4.
[7] with maximum absolute diff |7-7| = 0 <= 4. 
Therefore, the size of the longest subarray is 2.
```

**Example 2**

```
Input: nums = [10,1,2,4,7,2], limit = 5
Output: 4 
Explanation: The subarray [2,4,7,2] is the longest since the maximum absolute diff is |2-7| = 5 <= 5.
```

**Example 3**

```
Input: nums = [4,2,2,2,4,4,2,2], limit = 0
Output: 3
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

import java.util.Deque;
import java.util.LinkedList;

/**
 * @author zhengxingxing
 * @date 2025/02/16
 */
public class LongestContinuousSubarrayWithAbsoluteDiffLessThanOrEqualToLimit {
    
    public static int longestSubarray(int[] nums, int limit) {
        // Monotonic decreasing deque to maintain window maximum
        // Elements are stored in decreasing order, so first element is always the maximum
        Deque<Integer> maxDeque = new LinkedList<>();

        // Monotonic increasing deque to maintain window minimum
        // Elements are stored in increasing order, so first element is always the minimum
        Deque<Integer> minDeque = new LinkedList<>();

        // Left pointer of the sliding window
        int left = 0;
        // Length of the longest valid subarray found so far
        int maxLen = 0;

        // Right pointer iterates through the array
        for (int right = 0; right < nums.length; right++) {
            // Maintain the monotonic decreasing property of maxDeque
            // Remove all elements smaller than the current element from the back
            // This ensures that maxDeque.peekFirst() always gives the maximum element
            while (!maxDeque.isEmpty() && nums[right] > maxDeque.peekLast()) {
                maxDeque.pollLast();
            }
            maxDeque.offerLast(nums[right]);

            // Maintain the monotonic increasing property of minDeque
            // Remove all elements larger than the current element from the back
            // This ensures that minDeque.peekFirst() always gives the minimum element
            while (!minDeque.isEmpty() && nums[right] < minDeque.peekLast()) {
                minDeque.pollLast();
            }
            minDeque.offerLast(nums[right]);

            // Shrink the window if the difference between max and min exceeds limit
            // Keep shrinking until the window becomes valid or empty
            while (maxDeque.peekFirst() - minDeque.peekFirst() > limit) {
                // If the leftmost element is the current maximum, remove it from maxDeque
                if (nums[left] == maxDeque.peekFirst()) {
                    maxDeque.pollFirst();
                }
                // If the leftmost element is the current minimum, remove it from minDeque
                if (nums[left] == minDeque.peekFirst()) {
                    minDeque.pollFirst();
                }
                // Move the left pointer to shrink the window
                left++;
            }

            // Update the maximum length if current window is valid
            // Current window size is (right - left + 1)
            maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen;
    }

    
    public static void main(String[] args) {
        // Test Case 1: Expected output: 2
        // Valid subarrays are [2,4] and [4,7] with max diff <= 4
        int[] nums1 = {8, 2, 4, 7};
        int limit1 = 4;
        System.out.println(longestSubarray(nums1, limit1));

        // Test Case 2: Expected output: 4
        // Longest valid subarray is [2,4,7,2] with max diff <= 5
        int[] nums2 = {10, 1, 2, 4, 7, 2};
        int limit2 = 5;
        System.out.println(longestSubarray(nums2, limit2));

        // Test Case 3: Expected output: 3
        // Longest valid subarray contains equal elements (either [2,2,2] or [4,4,4])
        int[] nums3 = {4, 2, 2, 2, 4, 4, 2, 2};
        int limit3 = 0;
        System.out.println(longestSubarray(nums3, limit3));
    }
}

```





