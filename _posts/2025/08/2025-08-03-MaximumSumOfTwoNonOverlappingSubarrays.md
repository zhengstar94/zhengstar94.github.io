---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1031. Maximum Sum of Two Non-Overlapping Subarrays"
date: "2025-08-03"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---



- Given an integer array `nums` and two integers `firstLen` and `secondLen`, return *the maximum sum of elements in two non-overlapping **subarrays** with lengths* `firstLen` *and* `secondLen`.
- The array with length `firstLen` could occur before or after the array with length `secondLen`, but they have to be non-overlapping.
- A **subarray** is a **contiguous** part of an array.

**Example 1**

```
Input: nums = [0,6,5,2,2,5,1,9,4], firstLen = 1, secondLen = 2
Output: 20
Explanation: One choice of subarrays is [9] with length 1, and [6,5] with length 2.
```

**Example 2**

```
Input: nums = [3,8,1,3,2,1,8,9,0], firstLen = 3, secondLen = 2
Output: 29
Explanation: One choice of subarrays is [3,8,1] with length 3, and [8,9] with length 2.
```

**Example 3**

```
Input: nums = [2,1,5,6,0,9,5,0,3,8], firstLen = 4, secondLen = 3
Output: 31
Explanation: One choice of subarrays is [5,6,0,9] with length 4, and [0,3,8] with length 3.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @Author zhengxingxing
 * @Date 2025/08/03
 */
public class MaximumSumOfTwoNonOverlappingSubarrays {

    public static int maxSumTwoNoOverlap(int[] nums, int firstLen, int secondLen) {
        // Try both possible orders: firstLen before secondLen, and secondLen before firstLen.
        // Return the maximum result from both orders.
        return Math.max(
                maxSum(nums, firstLen, secondLen),
                maxSum(nums, secondLen, firstLen)
        );
    }


    private static int maxSum(int[] nums, int L, int M) {
        int n = nums.length;
        // prefix[i] stores the sum of nums[0] to nums[i-1].
        // This allows us to quickly calculate the sum of any subarray in O(1) time.
        int[] prefix = new int[n + 1];
        for (int i = 0; i < n; i++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }

        int maxL = 0; // The maximum sum of any subarray of length L that ends before the current M-length window.
        int res = 0;  // The overall maximum sum of two non-overlapping subarrays.

        // Start from i = L + M, so that both L-length and M-length subarrays are fully within the range.
        // i represents the right boundary (exclusive) of the current M-length subarray.
        for (int i = L + M; i <= n; i++) {
            // Calculate the sum of the L-length subarray that ends right before the current M-length subarray.
            // prefix[i - M] - prefix[i - M - L] gives the sum of nums[(i-M-L) ... (i-M-1)].
            // We keep track of the maximum such sum seen so far in maxL.
            maxL = Math.max(maxL, prefix[i - M] - prefix[i - M - L]);

            // Calculate the sum of the current M-length subarray: nums[(i-M) ... (i-1)].
            // prefix[i] - prefix[i - M] gives this sum.
            // Add it to maxL (the best L-length subarray sum before this window) to get the total sum.
            // Update res if this total sum is greater than the previous maximum.
            res = Math.max(res, maxL + prefix[i] - prefix[i - M]);
        }

        return res;
    }

    public static void main(String[] args) {
        int[] nums1 = {0,6,5,2,2,5,1,9,4};
        System.out.println(maxSumTwoNoOverlap(nums1, 1, 2)); // Output: 20

        int[] nums2 = {3,8,1,3,2,1,8,9,0};
        System.out.println(maxSumTwoNoOverlap(nums2, 3, 2)); // Output: 29

        int[] nums3 = {2,1,5,6,0,9,5,0,3,8};
        System.out.println(maxSumTwoNoOverlap(nums3, 4, 3)); // Output: 31
    }
}

```





