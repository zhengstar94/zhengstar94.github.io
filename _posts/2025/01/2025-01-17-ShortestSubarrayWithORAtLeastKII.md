---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "(Review)3097. Shortest Subarray With OR at Least K II"
date: "2025-01-17"
tags: Medium Review SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowMax"
---


- You are given an array `nums` of **non-negative** integers and an integer `k`.
- An array is called **special** if the bitwise `OR` of all of its elements is **at least** `k`
- Return *the length of the **shortest** **special** **non-empty*** *subarray* *of* `nums`, *or return* `-1` *if no special subarray exists*.

**Example 1**

```
Input: nums = [1,2,3], k = 2

Output: 1

Explanation:

The subarray [3] has OR value of 3. Hence, we return 1.
```

**Example 2**

```
Input: nums = [2,1,8], k = 10

Output: 3

Explanation:

The subarray [2,1,8] has OR value of 11. Hence, we return 3.
```

**Example 3**

```
Input: nums = [1,2], k = 0

Output: 1

Explanation:

The subarray [1] has OR value of 1. Hence, we return 1.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/17
 */
public class ShortestSubarrayWithORAtLeastKII {
    public static int minimumSubarrayLength(int[] nums, int k) {
        // Initialize answer with maximum value to track minimum length
        int ans = Integer.MAX_VALUE;
        // Left pointer of sliding window
        int left = 0;
        // Bottom marker to track the last position where OR values were properly calculated
        int bottom = 0;
        // Variable to store the running OR value of current window
        int rightOr = 0;

        // Iterate through array with right pointer
        for (int right = 0; right < nums.length; right++) {
            // Update running OR value by including current element
            rightOr |= nums[right];

            // Try to minimize window size while maintaining OR >= k condition
            while (left <= right && (nums[left] | rightOr) >= k) {
                // Update minimum length found so far
                ans = Math.min(ans, right - left + 1);
                // Move left pointer to try finding smaller valid window
                left++;

                // If left pointer moves beyond bottom marker, need to recalculate OR values
                if (bottom < left) {
                    // Rebuild OR values from right to left
                    // This is necessary because moving left pointer affects the OR values
                    // We store cumulative OR values in the original array
                    for (int i = right - 1; i >= left; i--) {
                        // Update each position with its OR value with the next element
                        nums[i] |= nums[i + 1];
                    }

                    // Update bottom marker to current right position
                    // This indicates we've recalculated OR values up to this point
                    bottom = right;
                    // Reset rightOr as the OR values are now stored in the array
                    rightOr = 0;
                }
            }
        }

        // Return -1 if no valid subarray found, otherwise return minimum length
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }


    public static void main(String[] args) {
        // Test Case 1: Expected output is 1
        int[] nums1 = {1, 2, 3};
        int k1 = 2;
        System.out.println("Test Case 1 Result: " + minimumSubarrayLength(nums1, k1));

        // Test Case 2: Expected output is 3
        int[] nums2 = {2, 1, 8};
        int k2 = 10;
        System.out.println("Test Case 2 Result: " + minimumSubarrayLength(nums2, k2));

        // Test Case 3: Expected output is 1
        int[] nums3 = {1, 2};
        int k3 = 0;
        System.out.println("Test Case 3 Result: " + minimumSubarrayLength(nums3, k3));
    }
}

```





