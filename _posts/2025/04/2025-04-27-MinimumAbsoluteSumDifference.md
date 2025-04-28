---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1818. Minimum Absolute Sum Difference"
date: "2025-04-27"
tags: Medium
categories:
  - "LeetCode BinarySearch"
---


- You are given two positive integer arrays `nums1` and `nums2`, both of length `n`.
- The **absolute sum difference** of arrays `nums1` and `nums2` is defined as the **sum** of `|nums1[i] - nums2[i]|` for each `0 <= i < n` (**0-indexed**).
- You can replace **at most one** element of `nums1` with **any** other element in `nums1` to **minimize** the absolute sum difference.
- Return the *minimum absolute sum difference **after** replacing at most one element in the array `nums1`.* Since the answer may be large, return it **modulo** `10^9 + 7`.
- `|x|` is defined as:
  - `x` if `x >= 0`, or
  - `-x` if `x < 0`.

**Example 1**

```
Input: nums1 = [1,7,5], nums2 = [2,3,5]
Output: 3
Explanation: There are two possible optimal solutions:
- Replace the second element with the first: [1,7,5] => [1,1,5], or
- Replace the second element with the third: [1,7,5] => [1,5,5].
Both will yield an absolute sum difference of |1-2| + (|1-3| or |5-3|) + |5-5| = 3.
```

**Example 2**

```
Input: nums1 = [2,4,6,8,10], nums2 = [2,4,6,8,10]
Output: 0
Explanation: nums1 is equal to nums2 so no replacement is needed. This will result in an 
absolute sum difference of 0.
```

**Example 3**

```
Input: nums1 = [1,10,4,4,2,7], nums2 = [9,3,5,1,7,4]
Output: 20
Explanation: Replace the first element with the second: [1,10,4,4,2,7] => [10,10,4,4,2,7].
This yields an absolute sum difference of |10-9| + |10-3| + |4-5| + |4-1| + |2-7| + |7-4| = 20
```

## Method 1

```tex
【O(nlogn) time | O(n) space】
```

```java
package Leetcode.BinarySearch;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/04/27
 */
public class MinimumAbsoluteSumDifference {
    // Modulo constant to handle large numbers
    private static final int MOD = 1_000_000_007;

    public static int minAbsoluteSumDiff(int[] nums1, int[] nums2) {
        int n = nums1.length;
        // Create a sorted copy of nums1 for binary search
        int[] sortedNums1 = Arrays.copyOf(nums1, n);
        Arrays.sort(sortedNums1);

        // Calculate the initial total absolute difference
        long totalDiff = 0;
        for (int i = 0; i < n; i++) {
            totalDiff += Math.abs(nums1[i] - nums2[i]);
        }

        // Find the maximum improvement possible by replacing one element
        long maxImprovement = 0;
        for (int i = 0; i < n; i++) {
            // Calculate current absolute difference at position i
            int originalDiff = Math.abs(nums1[i] - nums2[i]);

            // Find the closest value in sorted nums1 to nums2[i]
            int closest = findClosest(sortedNums1, nums2[i]);
            // Calculate new difference if we replace with the closest value
            int newDiff = Math.abs(closest - nums2[i]);

            // Update maximum improvement if current replacement gives better result
            maxImprovement = Math.max(maxImprovement, originalDiff - newDiff);
        }

        // Return final result: (original sum - maximum improvement) % MOD
        return (int)((totalDiff - maxImprovement) % MOD);
    }

    private static int findClosest(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;

        // Handle edge cases where target is outside array bounds
        if (target <= arr[left]) {
            return arr[left];  // Target is smaller than or equal to smallest element
        }
        if (target >= arr[right]) {
            return arr[right]; // Target is larger than or equal to largest element
        }

        // Binary search process
        while (left <= right) {
            int mid = left + (right - left) / 2; // Avoid potential overflow

            if (arr[mid] == target) {
                return arr[mid]; // Exact match found
            } else if (arr[mid] < target) {
                left = mid + 1;  // Search in right half
            } else {
                right = mid - 1; // Search in left half
            }
        }

        // Compare the two closest values and return the one with smaller absolute difference
        // left and right have crossed, so arr[right] < target < arr[left]
        return Math.abs(arr[right] - target) <= Math.abs(arr[left] - target) ?
                arr[right] : arr[left];
    }

    public static void main(String[] args) {
        // Test Case 1: Basic case with replacement needed
        int[] nums1_1 = {1,7,5};
        int[] nums2_1 = {2,3,5};
        System.out.println("Test Case 1 Result: " + minAbsoluteSumDiff(nums1_1, nums2_1));
        // Expected output: 3

        // Test Case 2: Arrays are identical, no replacement needed
        int[] nums1_2 = {2,4,6,8,10};
        int[] nums2_2 = {2,4,6,8,10};
        System.out.println("Test Case 2 Result: " + minAbsoluteSumDiff(nums1_2, nums2_2));
        // Expected output: 0

        // Test Case 3: Complex case with larger numbers
        int[] nums1_3 = {1,10,4,4,2,7};
        int[] nums2_3 = {9,3,5,1,7,4};
        System.out.println("Test Case 3 Result: " + minAbsoluteSumDiff(nums1_3, nums2_3));
        // Expected output: 20
    }
}

```





