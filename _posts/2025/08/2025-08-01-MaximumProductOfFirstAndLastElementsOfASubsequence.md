---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3584. Maximum Product of First and Last Elements of a Subsequence"
date: "2025-08-01"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---


- You are given an integer array `nums` and an integer `m`.
- Return the **maximum** product of the first and last elements of any **subsequence** of `nums` of size `m`.

**Example 1**

```
Input: nums = [-1,-9,2,3,-2,-3,1], m = 1

Output: 81

Explanation:

The subsequence [-9] has the largest product of the first and last elements: -9 * -9 = 81. Therefore, the answer is 81.
```

**Example 2**

```
Input: nums = [1,3,-5,5,6,-4], m = 3

Output: 20

Explanation:

The subsequence [-5, 6, -4] has the largest product of the first and last elements.
```

**Example 3**

```
Input: nums = [2,-1,2,-6,5,2,-5,7], m = 2

Output: 35

Explanation:

The subsequence [5, 7] has the largest product of the first and last elements.
```

## Method 1

```tex
【O(n - m) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @Author zhengxingxing
 * @Date 2025/08/01
 */
public class MaximumProductOfFirstAndLastElementsOfASubsequence {

    public static long maximumProduct(int[] nums, int m) {
        // Initialize the answer to the smallest possible value.
        long ans = Long.MIN_VALUE;
        // min will keep track of the minimum value among all possible "first" elements for the current window.
        int min = Integer.MAX_VALUE;
        // max will keep track of the maximum value among all possible "first" elements for the current window.
        int max = Integer.MIN_VALUE;

        // Iterate over all possible positions for the last element of the subsequence.
        // i is the index of the current "last" element of the subsequence.
        for (int i = m - 1; i < nums.length; i++) {
            // y is the candidate for the "first" element of the subsequence.
            // For each i, the possible "first" elements are nums[0] to nums[i - m + 1].
            int y = nums[i - m + 1];

            // Update min and max to always reflect the smallest and largest possible "first" elements so far.
            min = Math.min(min, y);
            max = Math.max(max, y);

            // x is the current "last" element of the subsequence.
            long x = nums[i];

            // Calculate the product of the current "last" element with both the minimum and maximum "first" elements.
            // This is necessary because both large positive and large negative numbers can yield the largest product.
            long productWithMin = x * min;
            long productWithMax = x * max;

            // Update the answer with the maximum product found so far.
            ans = Math.max(ans, Math.max(productWithMin, productWithMax));
        }

        // Return the maximum product found.
        return ans;
    }

    public static void main(String[] args) {
        int[] nums1 = {-1, -9, 2, 3, -2, -3, 1};
        int m1 = 1;
        // Example 1: Should print 81
        System.out.println(maximumProduct(nums1, m1));

        int[] nums2 = {1, 3, -5, 5, 6, -4};
        int m2 = 3;
        // Example 2: Should print 20
        System.out.println(maximumProduct(nums2, m2));

        int[] nums3 = {2, -1, 2, -6, 5, 2, -5, 7};
        int m3 = 2;
        // Example 3: Should print 35
        System.out.println(maximumProduct(nums3, m3));
    }
}

```





