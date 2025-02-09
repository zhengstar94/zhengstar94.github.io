---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2302. Count Subarrays With Score Less Than K"
date: "2025-02-09"
tags: Hard SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowCountShortest"
---


- The **score** of an array is defined as the **product** of its sum and its length.
  - For example, the score of `[1, 2, 3, 4, 5]` is `(1 + 2 + 3 + 4 + 5) * 5 = 75`.
- Given a positive integer array `nums` and an integer `k`, return *the **number of non-empty subarrays** of* `nums` *whose score is **strictly less** than* `k`.
- A **subarray** is a contiguous sequence of elements within an array.

**Example 1**

```
Input: nums = [2,1,4,3,5], k = 10
Output: 6
Explanation:
The 6 subarrays having scores less than 10 are:
- [2] with score 2 * 1 = 2.
- [1] with score 1 * 1 = 1.
- [4] with score 4 * 1 = 4.
- [3] with score 3 * 1 = 3. 
- [5] with score 5 * 1 = 5.
- [2,1] with score (2 + 1) * 2 = 6.
Note that subarrays such as [1,4] and [4,3,5] are not considered because their scores are 10 and 36 respectively, while we need scores strictly less than 10.
```

**Example 2**

```
Input: nums = [1,1,1], k = 5
Output: 5
Explanation:
Every subarray except  [1,1,1] has a score less than 5.
[1,1,1] has a score (1 + 1 + 1) * 3 = 9, which is greater than 5.
Thus, there are 5 subarrays having scores less than 5.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowCountShortest;

/**
 * @author zhengxingxing
 * @date 2025/02/09
 */
public class CountSubarraysWithScoreLessThanK {
    
    public static long countSubarrays(int[] nums, long k) {
        // Initialize result counter for valid subarrays
        long result = 0;
        // Track running sum of current window
        long sum = 0;
        // Left pointer of sliding window
        int left = 0;

        // Iterate through array using right pointer
        for (int right = 0; right < nums.length; right++) {
            // Add current element to window sum
            sum += nums[right];

            // Shrink window while score is >= k
            // Window score = sum * window_length
            while (sum * (right - left + 1) >= k) {
                // Remove leftmost element from sum
                sum -= nums[left];
                // Move left pointer to shrink window
                left++;
            }

            // Add count of valid subarrays ending at current right pointer
            // All subarrays from left to right are valid
            result += (right - left + 1);
        }

        return result;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Basic case with mixed numbers
        int[] nums1 = {2, 1, 4, 3, 5};
        long k1 = 10;
        System.out.println("Test Case 1 Result: " + countSubarrays(nums1, k1)); // Expected: 6

        // Test Case 2: Array with identical elements
        int[] nums2 = {1, 1, 1};
        long k2 = 5;
        System.out.println("Test Case 2 Result: " + countSubarrays(nums2, k2)); // Expected: 5

        // Test Case 3: Large array performance test
        // Creates array of size 100000 filled with 1's
        int[] nums3 = new int[100000];
        for (int i = 0; i < nums3.length; i++) {
            nums3[i] = 1;
        }
        long k3 = 50;
        System.out.println("Test Case 3 Result: " + countSubarrays(nums3, k3)); // Performance test

        // Test Case 4: Single element array
        int[] nums4 = {10};
        long k4 = 15;
        System.out.println("Test Case 4 Result: " + countSubarrays(nums4, k4)); // Expected: 1

        // Test Case 5: No valid subarrays
        int[] nums5 = {10, 20, 30};
        long k5 = 1;
        System.out.println("Test Case 5 Result: " + countSubarrays(nums5, k5)); // Expected: 0
    }
}

```





