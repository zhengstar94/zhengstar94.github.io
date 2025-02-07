---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3095. Shortest Subarray With OR at Least K I"
date: "2025-01-16"
tags: Easy SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowMax"
---

- You are given an array `nums` of **non-negative** integers and an integer `k`.
- An array is called **special** if the bitwise `OR` of all of its elements is **at least** `k`.
- Return *the length of the **shortest** **special** **non-empty*** *subarray* *of* `nums`, *or return* `-1` *if no special subarray exists*.

**Example 1**

```
Input: nums = [1,2,3], k = 2

Output: 1

Explanation:

The subarray [3] has OR value of 3. Hence, we return 1.

Note that [2] is also a special subarray.
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
【O(n^2) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/01/16
 */
public class ShortestSubarrayWithORAtLeastKI {
    public static int minimumSubarrayLength(int[] nums, int k) {
        // Get the length of input array
        int n = nums.length;
        // Return -1 if array is empty
        if (n == 0) {
            return -1;
        }

        // Initialize answer with maximum integer value
        int ans = Integer.MAX_VALUE;

        // Outer loop: iterate through all possible starting positions
        for (int i = 0; i < n; i++) {
            // Initialize OR sum for current subarray
            int orSum = 0;
            // Inner loop: extend subarray and calculate OR sum
            for (int j = i; j < n; j++) {
                // Update OR sum by including current element
                orSum |= nums[j];
                // If OR sum reaches or exceeds k, update minimum length
                if (orSum >= k) {
                    ans = Math.min(ans, j - i + 1);
                    break;  // Found valid subarray, move to next starting position
                }
            }
        }

        // Return -1 if no valid subarray found, otherwise return minimum length
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }

    public static void main(String[] args) {
        // Test Case 1: Simple case with small numbers
        int[] nums1 = {1, 2, 3};
        int k1 = 2;
        System.out.println("Test Case 1 Result: " + minimumSubarrayLength(nums1, k1)); // Expected output: 1

        // Test Case 2: Case requiring multiple elements
        int[] nums2 = {2, 1, 8};
        int k2 = 10;
        System.out.println("Test Case 2 Result: " + minimumSubarrayLength(nums2, k2)); // Expected output: 3

        // Test Case 3: Case with k = 0
        int[] nums3 = {1, 2};
        int k3 = 0;
        System.out.println("Test Case 3 Result: " + minimumSubarrayLength(nums3, k3)); // Expected output: 1

        // Test Case 4: Case with no valid solution
        int[] nums4 = {5, 5, 5, 5};
        int k4 = 7;
        System.out.println("Test Case 4 Result: " + minimumSubarrayLength(nums4, k4)); // Expected output: -1
    }
}

```





