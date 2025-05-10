---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2918. Minimum Equal Sum of Two Arrays After Replacing Zeros"
date: "2025-05-10"
tags: Medium
categories:
  - "LeetCode Greedy"
---


- You are given two arrays `nums1` and `nums2` consisting of positive integers.
- You have to replace **all** the `0`'s in both arrays with **strictly** positive integers such that the sum of elements of both arrays becomes **equal**.
- Return *the **minimum** equal sum you can obtain, or* `-1` *if it is impossible*.

**Example 1**

```
Input: nums1 = [3,2,0,1,0], nums2 = [6,5,0]
Output: 12
Explanation: We can replace 0's in the following way:
- Replace the two 0's in nums1 with the values 2 and 4. The resulting array is nums1 = [3,2,2,1,4].
- Replace the 0 in nums2 with the value 1. The resulting array is nums2 = [6,5,1].
Both arrays have an equal sum of 12. It can be shown that it is the minimum sum we can obtain.
```

**Example 2**

```
Input: nums1 = [2,0,2,0], nums2 = [1,4]
Output: -1
Explanation: It is impossible to make the sum of both arrays equal.
```

## Method 1

```tex
【O(n + m) time | O(1) space】
```

```java
package Leetcode.Greedy;

/**
 * Author: zhengxingxing
 * Date: 2025/05/10
 */
public class MinimumEqualSumOfTwoArraysAfterReplacingZeros {

    public static long minimumEqualSum(int[] nums1, int[] nums2) {
        long sum1 = 0;
        long sum2 = 0;

        int zero1 = 0;
        int zero2 = 0;

        // Calculate the sum and count the number of zeros in nums1
        for (int num : nums1) {
            sum1 += num;
            if (num == 0) {
                zero1++;
            }
        }

        // Calculate the sum and count the number of zeros in nums2
        for (int num : nums2) {
            sum2 += num;
            if (num == 0) {
                zero2++;
            }
        }

        // The minimum possible sum after replacing all zeros with 1
        long minSum1 = sum1 + zero1; // Each zero in nums1 is replaced by 1
        long minSum2 = sum2 + zero2; // Each zero in nums2 is replaced by 1

        /**
         * Case 1:
         * If both arrays already have the same sum AND there are no zeros,
         * it means the arrays are naturally equal without needing any replacements.
         * We can directly return the sum as the result.
         *
         * Example:
         * nums1 = {1, 2, 3}
         * nums2 = {3, 2, 1}
         * Both have sum = 6 and no zeros -> return 6.
         */
        if (sum1 == sum2 && zero1 == 0 && zero2 == 0) {
            return sum1;
        }

        /**
         * Case 2:
         * If the minimum possible sum of nums1 is greater than that of nums2,
         * and nums2 has zeros, we can potentially increase nums2's sum by
         * replacing its zeros with larger numbers to match nums1's sum.
         * We return the minSum1 as the minimum target.
         */
        if (minSum1 > minSum2 && zero2 > 0) {
            return minSum1;
        }

        /**
         * Case 3:
         * Similarly, if the minimum possible sum of nums2 is greater than that of nums1,
         * and nums1 has zeros, we can potentially increase nums1's sum by
         * replacing its zeros with larger numbers to match nums2's sum.
         * We return the minSum2 as the minimum target.
         */
        if (minSum2 > minSum1 && zero1 > 0) {
            return minSum2;
        }

        /**
         * Case 4:
         * This is a subtle and important case:
         * If both arrays' minimum possible sums (after replacing all zeros with 1)
         * are already equal, it means they can be balanced by just replacing all
         * zeros with 1 and nothing more needs to be done.
         *
         * Example:
         * nums1 = {0, 2, 3}  -> sum1 = 5, zero1 = 1, minSum1 = 6
         * nums2 = {1, 0, 4}  -> sum2 = 5, zero2 = 1, minSum2 = 6
         * Both minSum1 and minSum2 == 6 -> return 6.
         */
        if (minSum1 == minSum2) {
            return minSum1;
        }

        /**
         * If none of the above cases apply, it's impossible to make the arrays equal,
         * even after replacing zeros.
         */
        return -1;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {3, 2, 0, 1, 0};
        int[] nums2 = {6, 5, 0};
        System.out.println("Test case 1 result: " + minimumEqualSum(nums1, nums2)); // Expected: 12

        // Test case 2
        int[] nums1Case2 = {2, 0, 2, 0};
        int[] nums2Case2 = {1, 4};
        System.out.println("Test case 2 result: " + minimumEqualSum(nums1Case2, nums2Case2)); // Expected: -1

        // Additional test case 3
        int[] nums1Case3 = {0, 0, 0};
        int[] nums2Case3 = {0, 0, 0};
        System.out.println("Test case 3 result: " + minimumEqualSum(nums1Case3, nums2Case3)); // Expected: 3

        // Additional test case 4
        int[] nums1Case4 = {1, 2, 3};
        int[] nums2Case4 = {1, 2, 3};
        System.out.println("Test case 4 result: " + minimumEqualSum(nums1Case4, nums2Case4)); // Expected: 6
    }
}
```





