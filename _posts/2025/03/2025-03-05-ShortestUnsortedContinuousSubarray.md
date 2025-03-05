---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "581. Shortest Unsorted Continuous Subarray"
date: "2025-03-05"
tags: Medium TwoPointers
categories:
  - "LeetCode SingleSeqTwoPointersForward" 
---

- Given an integer array `nums`, you need to find one **continuous subarray** such that if you only sort this subarray in non-decreasing order, then the whole array will be sorted in non-decreasing order.
- Return *the shortest such subarray and output its length*.

**Example 1**

```
Input: nums = [2,6,4,8,10,9,15]
Output: 5
Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.
```

**Example 2**

```
Input: nums = [1,2,3,4]
Output: 0
```

**Example 3**

```
Input: nums = [1]
Output: 0
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer.SingleSeqTwoPointersForward;

/**
 * @author zhengxingxing
 * @date 2025/03/05
 */
public class ShortestUnsortedContinuousSubarray {

    public static int findUnsortedSubarray(int[] nums) {
        int n = nums.length;
        // Handle base cases of empty array or single element
        if (n <= 1) {
            return 0;
        }

        // Find left boundary - first element that breaks ascending order
        int left = 0;
        while (left < n - 1 && nums[left] <= nums[left + 1]) {
            left++;
        }

        // If array is already sorted, return 0
        if (left == n - 1) {
            return 0;
        }

        // Find right boundary - first element from right that breaks descending order
        int right = n - 1;
        while (right > 0 && nums[right] >= nums[right - 1]) {
            right--;
        }

        // Find min and max values in the unsorted subarray
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
        for (int i = left; i <= right; i++) {
            min = Math.min(min, nums[i]);
            max = Math.max(max, nums[i]);
        }

        // Expand left boundary - ensure all elements to the left are <= min
        while (left > 0 && nums[left - 1] > min) {
            left--;
        }

        // Expand right boundary - ensure all elements to the right are >= max
        while (right < n - 1 && nums[right + 1] < max) {
            right++;
        }

        // Return length of unsorted subarray
        return right - left + 1;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Array with unsorted middle portion
        int[] nums1 = {2,6,4,8,10,9,15};
        System.out.println("Test Case 1 Input: [2,6,4,8,10,9,15]");
        System.out.println("Output: " + findUnsortedSubarray(nums1));

        // Test Case 2: Already sorted array
        int[] nums2 = {1,2,3,4};
        System.out.println("\nTest Case 2 Input: [1,2,3,4]");
        System.out.println("Output: " + findUnsortedSubarray(nums2));

        // Test Case 3: Single element array
        int[] nums3 = {1};
        System.out.println("\nTest Case 3 Input: [1]");
        System.out.println("Output: " + findUnsortedSubarray(nums3));

        // Test Case 4: Array with duplicate elements
        int[] nums4 = {1,3,2,2,2};
        System.out.println("\nTest Case 4 Input: [1,3,2,2,2]");
        System.out.println("Output: " + findUnsortedSubarray(nums4));
    }
}

```





