---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1283. Find the Smallest Divisor Given a Threshold"
date: "2025-04-28"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.FindMinimum"
---


- Given an array of integers `nums` and an integer `threshold`, we will choose a positive integer `divisor`, divide all the array by it, and sum the division's result. Find the **smallest** `divisor` such that the result mentioned above is less than or equal to `threshold`.
- Each result of the division is rounded to the nearest integer greater than or equal to that element. (For example: `7/3 = 3` and `10/2 = 5`).
- The test cases are generated so that there will be an answer.

**Example 1**

```
Input: nums = [1,2,5,9], threshold = 6
Output: 5
Explanation: We can get a sum to 17 (1+2+5+9) if the divisor is 1. 
If the divisor is 4 we can get a sum of 7 (1+1+2+3) and if the divisor is 5 the sum will be 5 (1+1+1+2). 
```

**Example 2**

```
Input: nums = [44,22,33,11,1], threshold = 5
Output: 44
```

## Method 1

```tex
【O(N * log(M)) time | O(1) space】
```

```java
package Leetcode.BinarySearch.FindMinimum;

/**
 * @author zhengxingxing
 * @date 2025/04/28
 */
public class FindTheSmallestDivisorGivenAThreshold {

    public static int smallestDivisor(int[] nums, int threshold) {
        // Find maximum number in array to set binary search upper bound
        int maxNum = 0;
        for (int num : nums) {
            maxNum = Math.max(maxNum, num);
        }

        // Perform binary search to find smallest valid divisor
        // Left boundary: 1 (minimum possible divisor)
        // Right boundary: maxNum (maximum needed divisor)
        int left = 1, right = maxNum;
        while (left < right) {
            // Calculate middle point avoiding overflow
            int mid = left + (right - left) / 2;

            // If current divisor produces sum <= threshold,
            // try to find smaller divisor in left half
            if (calculateSum(nums, mid) <= threshold) {
                right = mid;
            } else {
                // If sum > threshold, need larger divisor
                left = mid + 1;
            }
        }
        return left;
    }

    private static int calculateSum(int[] nums, int divisor) {
        int sum = 0;
        for (int num : nums) {
            // Calculate ceiling division using integer arithmetic
            // Formula: ceil(num/divisor) = (num + divisor - 1) / divisor
            sum += (num + divisor - 1) / divisor;
        }
        return sum;
    }

    public static void main(String[] args) {
        // Test Case 1: Basic example with small numbers
        int[] nums1 = {1, 2, 5, 9};
        System.out.println("Test Case 1: " + smallestDivisor(nums1, 6));  // Expected: 5

        // Test Case 2: Example with prime numbers
        int[] nums2 = {2, 3, 5, 7, 11};
        System.out.println("Test Case 2: " + smallestDivisor(nums2, 11));  // Expected: 3

        // Test Case 3: Single element array
        int[] nums3 = {19};
        System.out.println("Test Case 3: " + smallestDivisor(nums3, 5));  // Expected: 4
    }
}

```





