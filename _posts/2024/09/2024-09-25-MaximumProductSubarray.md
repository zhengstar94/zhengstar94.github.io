---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "152.Maximum Product Subarray"
date: "2024-09-25"
categories:
  - "LeetCode Dynamic Programming"
---

- Given an integer array `nums`, find a subarray that has the largest product, and return *the product*.
- The test cases are generated so that the answer will fit in a **32-bit** integer.

**Example 1**

```
Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

**Example 2**

```
Input: nums = [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * @author zhengstars
 * @date 2024/09/25
 */
public class MaximumProductSubarray {
    
    public static int maxProduct(int[] nums) {
        // Edge case: if the array is null or empty, return 0
        if (nums == null || nums.length == 0) {
            return 0;
        }

        // Initialize max as the first element of the array
        int max = nums[0];
        // imax/imin stores the max/min product of subarray that ends with the current number
        int imax = 1;
        int imin = 1;

        // Iterate through the array
        for (int i = 0; i < nums.length; i++) {
            // If we encounter a negative number, swap imax and imin
            // This is because a negative number will make the bigger number smaller and the smaller number bigger
            if (nums[i] < 0) {
                int tmp = imax;
                imax = imin;
                imin = tmp;
            }

            // Update imax: compare current number with product of current number and previous imax
            imax = Math.max(imax * nums[i], nums[i]);
            // Update imin: compare current number with product of current number and previous imin
            imin = Math.min(imin * nums[i], nums[i]);

            // Update max if we have found a larger product
            max = Math.max(max, imax);
        }

        return max;
    }

    public static void main(String[] args) {

        // Test case 1: Normal case
        int[] nums1 = {2, 3, -2, 4};
        System.out.println("Test case 1 result: " + maxProduct(nums1)); // Expected output: 6

        // Test case 2: All positive numbers
        int[] nums2 = {1, 2, 3, 4};
        System.out.println("Test case 2 result: " + maxProduct(nums2)); // Expected output: 24

        // Test case 3: All negative numbers
        int[] nums3 = {-1, -2, -3};
        System.out.println("Test case 3 result: " + maxProduct(nums3)); // Expected output: 6

        // Test case 4: Mixed positive and negative with zero
        int[] nums4 = {-2, 0, -1};
        System.out.println("Test case 4 result: " + maxProduct(nums4)); // Expected output: 0

        // Test case 5: Single element array
        int[] nums5 = {-5};
        System.out.println("Test case 5 result: " + maxProduct(nums5)); // Expected output: -5

        // Test case 6: Empty array
        int[] nums6 = {};
        System.out.println("Test case 6 result: " + maxProduct(nums6)); // Expected output: 0
    }
}

```





