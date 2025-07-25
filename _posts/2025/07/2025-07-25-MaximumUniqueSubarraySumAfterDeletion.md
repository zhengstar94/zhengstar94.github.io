---
toc:
beginning: true
giscus_comments: true
layout: post
title: "3487. Maximum Unique Subarray Sum After Deletion"
date: "2025-07-25"
tags: Easy
categories:
    - "LeetCode Greedy"
---


- You are given an integer array `nums`.
- You are allowed to delete any number of elements from `nums` without making it **empty**. After performing the deletions, select a subarray of `nums` such that:
    - All elements in the subarray are **unique**.
    - The sum of the elements in the subarray is **maximized**.
- Return the **maximum sum** of such a subarray.

**Example 1**

```
Input: nums = [1,2,3,4,5]
Output: 15

Explanation:

Select the entire array without deleting any element to obtain the maximum sum.
```

**Example 2**

```
Input: nums = [1,1,0,1,1]
Output: 1

Explanation:

Delete the element nums[0] == 1, nums[1] == 1, nums[2] == 0, and nums[3] == 1. Select the entire array [1] to obtain the maximum sum.
```

**Example 3**

```
Input: nums = [1,2,-1,-2,1,0,-1]
Output: 3

Explanation:

Delete the elements nums[2] == -1 and nums[3] == -2, and select the subarray [2, 1] from [1, 2, 1, 0, -1] to obtain the maximum sum.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Greedy;

import java.util.HashSet;
import java.util.Set;

/**
 * @Author zhengxingxing
 * @Date 2025/07/25
 */
public class MaximumUniqueSubarraySumAfterDeletion {

    public static int maxSum(int[] nums) {
        // This set is used to store unique positive numbers encountered in the array.
        Set<Integer> set = new HashSet<>();
        // This variable accumulates the sum of all unique positive numbers.
        int sum = 0;
        // This variable keeps track of the largest negative number in the array.
        // It is initialized to the smallest possible integer value.
        int maxNegative = Integer.MIN_VALUE;

        // Traverse each element in the input array.
        for (int x : nums) {
            if (x < 0) {
                // If the current number is negative, update maxNegative if this number is larger.
                // This is important for the case where all numbers are negative.
                maxNegative = Math.max(maxNegative, x);
            } else if (set.add(x)) {
                // If the current number is non-negative and not already in the set (i.e., unique),
                // add it to the set and add its value to the sum.
                // Only the first occurrence of each positive number is counted.
                sum += x;
            }
            // If the number is non-negative but already in the set, it is skipped (no duplicate allowed).
        }

        // If the set is empty, it means there were no non-negative numbers in the array.
        // In this case, return the largest negative number found.
        // Otherwise, return the sum of all unique positive numbers.
        return set.isEmpty() ? maxNegative : sum;
    }

    public static void main(String[] args) {
        int[] nums1 = {1, 2, 3, 4, 5};
        int[] nums2 = {1, 1, 0, 1, 1};
        int[] nums3 = {1, 2, -1, -2, 1, 0, -1};
        int[] nums4 = {-3, -2, -5, -2};

        // Test case 1: All unique positive numbers, should return 15 (1+2+3+4+5)
        System.out.println(maxSum(nums1)); // Output: 15

        // Test case 2: Only one unique non-negative number (1), should return 1
        System.out.println(maxSum(nums2)); // Output: 1

        // Test case 3: Mixed numbers, unique non-negative numbers are 1, 2, 0, sum is 3
        System.out.println(maxSum(nums3)); // Output: 3

        // Test case 4: All negative numbers, should return the largest one (-2)
        System.out.println(maxSum(nums4)); // Output: -2
    }
}

```





