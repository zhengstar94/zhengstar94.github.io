---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3264. Final Array State After K Multiplication Operations I"
date: "2024-12-13"
tags: Easy
categories:
  - "LeetCode Array"
---


- You are given an integer array `nums`, an integer `k`, and an integer `multiplier`.

  You need to perform `k` operations on `nums`. In each operation:

  - Find the **minimum** value `x` in `nums`. If there are multiple occurrences of the minimum value, select the one that appears **first**.
  - Replace the selected minimum value `x` with `x * multiplier`.

- Return an integer array denoting the *final state* of `nums` after performing all `k` operations.

**Example 1**

```
Input: nums = [2,1,3,5,6], k = 5, multiplier = 2

Output: [8,4,6,5,6]

Explanation:

Operation	Result
After operation 1	[2, 2, 3, 5, 6]
After operation 2	[4, 2, 3, 5, 6]
After operation 3	[4, 4, 3, 5, 6]
After operation 4	[4, 4, 6, 5, 6]
After operation 5	[8, 4, 6, 5, 6]
```

**Example 2**

```
Input: nums = [1,2], k = 3, multiplier = 4

Output: [16,8]

Explanation:

Operation	Result
After operation 1	[4, 2]
After operation 2	[4, 8]
After operation 3	[16, 8]
```

## Method 1

```tex
【O(n * k) time | O(1) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2024/12/13
 */
public class FinalArrayStateAfterKMultiplicationOperationsI {
    public static int[] getFinalState(int[] nums, int k, int multiplier) {
        // Perform k multiplication operations
        for (int i = 0; i < k; i++) {
            // Initialize minimum value to maximum integer
            int min = Integer.MAX_VALUE;

            // Initialize index of minimum value
            int minIndex = 0;

            // Find the minimum value and its index in the array
            for (int j = 0; j < nums.length; j++) {
                // Update minimum value and its index if a smaller value is found
                if(nums[j] < min){
                    minIndex = j;
                    min = nums[j];
                }
            }

            // Multiply the minimum value by the multiplier
            nums[minIndex] = min * multiplier;
        }
        return nums;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {2, 1, 3, 5, 6};
        int k1 = 5;
        int multiplier1 = 2;
        int[] result1 = getFinalState(nums1, k1, multiplier1);
        System.out.println("Test Case 1 Result: " + Arrays.toString(result1));

        // Test case 2
        int[] nums2 = {1, 2};
        int k2 = 3;
        int multiplier2 = 4;
        int[] result2 = getFinalState(nums2, k2, multiplier2);
        System.out.println("Test Case 2 Result: " + Arrays.toString(result2));
    }
}

```

