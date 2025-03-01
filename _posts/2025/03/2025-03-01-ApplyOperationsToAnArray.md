---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2460. Apply Operations to an Array"
date: "2025-03-01"
tags: Easy
categories:
  - "LeetCode Array"
---


- You are given a **0-indexed** array `nums` of size `n` consisting of **non-negative** integers.
- You need to apply `n - 1` operations to this array where, in the `ith` operation (**0-indexed**), you will apply the following on the `ith` element of `nums`:
  - If `nums[i] == nums[i + 1]`, then multiply `nums[i]` by `2` and set `nums[i + 1]` to `0`. Otherwise, you skip this operation.
- After performing **all** the operations, **shift** all the `0`'s to the **end** of the array.
  - For example, the array `[1,0,2,0,0,1]` after shifting all its `0`'s to the end, is `[1,2,1,0,0,0]`.
- Return *the resulting array*.
- **Note** that the operations are applied **sequentially**, not all at once.

**Example 1**

```
Input: nums = [1,2,2,1,1,0]
Output: [1,4,2,0,0,0]
Explanation: We do the following operations:
- i = 0: nums[0] and nums[1] are not equal, so we skip this operation.
- i = 1: nums[1] and nums[2] are equal, we multiply nums[1] by 2 and change nums[2] to 0. The array becomes [1,4,0,1,1,0].
- i = 2: nums[2] and nums[3] are not equal, so we skip this operation.
- i = 3: nums[3] and nums[4] are equal, we multiply nums[3] by 2 and change nums[4] to 0. The array becomes [1,4,0,2,0,0].
- i = 4: nums[4] and nums[5] are equal, we multiply nums[4] by 2 and change nums[5] to 0. The array becomes [1,4,0,2,0,0].
After that, we shift the 0's to the end, which gives the array [1,4,2,0,0,0].
```

**Example 2**

```
Input: nums = [0,1]
Output: [1,0]
Explanation: No operation can be applied, we just shift the 0 to the end.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/03/01
 */
public class ApplyOperationsToAnArray {

    public static int[] applyOperations(int[] nums) {
        int n = nums.length;

        // Step 1: Apply multiplication operations
        // If adjacent elements are equal, multiply first by 2 and set second to 0
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                nums[i] *= 2;          // Double the current element
                nums[i + 1] = 0;       // Set next element to zero
            }
        }

        // Step 2: Move all non-zero elements to front using two-pointer technique
        // nonZero pointer keeps track of where next non-zero element should go
        int nonZero = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] != 0) {
                // Swap current element with element at nonZero position
                // When i equals nonZero, element swaps with itself (no effect)
                int temp = nums[i];
                nums[i] = nums[nonZero];
                nums[nonZero] = temp;
                nonZero++;    // Move nonZero pointer to next position
            }
        }
        return nums;
    }

    
    public static void main(String[] args) {
        // Test Case 1: Array with duplicates and zero
        // Expected: [1,4,2,0,0,0]
        int[] nums1 = {1, 2, 2, 1, 1, 0};
        System.out.println("Test Case 1:");
        System.out.println("Input: " + Arrays.toString(nums1));
        System.out.println("Output: " + Arrays.toString(applyOperations(nums1)));

        // Test Case 2: Minimal array with zero
        // Expected: [1,0]
        int[] nums2 = {0, 1};
        System.out.println("\nTest Case 2:");
        System.out.println("Input: " + Arrays.toString(nums2));
        System.out.println("Output: " + Arrays.toString(applyOperations(nums2)));

        // Test Case 3: Array with all same elements
        // Expected: [2,2,0,0]
        int[] nums3 = {1, 1, 1, 1};
        System.out.println("\nTest Case 3:");
        System.out.println("Input: " + Arrays.toString(nums3));
        System.out.println("Output: " + Arrays.toString(applyOperations(nums3)));
    }
}

```





