---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2873. Maximum Value of an Ordered Triplet I"
date: "2025-04-02"
tags: Easy
categories:
  - "LeetCode Array"
---

- You are given a **0-indexed** integer array `nums`.
- Return ***the maximum value over all triplets of indices*** `(i, j, k)` *such that* `i < j < k`. If all such triplets have a negative value, return `0`.
- The **value of a triplet of indices** `(i, j, k)` is equal to `(nums[i] - nums[j ] ) * nums[k]`.

**Example 1**

```
Input: nums = [12,6,1,2,7]
Output: 77
Explanation: The value of the triplet (0, 2, 4) is (nums[0] - nums[2 ] ) * nums[4] = 77.
It can be shown that there are no ordered triplets of indices with a value greater than 77. 
```

**Example 2**

```
Input: nums = [1,10,3,4,19]
Output: 133
Explanation: The value of the triplet (1, 2, 4) is (nums[1] - nums[2 ] ) * nums[4] = 133.
It can be shown that there are no ordered triplets of indices with a value greater than 133.
```

**Example 3**

```
Input: nums = [1,2,3]
Output: 0
Explanation: The only ordered triplet of indices (0, 1, 2) has a negative value of (nums[0] - nums[1 ] ) * nums[2] = -3. Hence, the answer would be 0.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/04/02
 */
public class MaximumValueOfAnOrderedTripletI {

    public static long maximumTripletValue(int[] nums) {
        // Initialize the answer to store the maximum triplet value
        long ans = 0;
        // Store the maximum difference (nums[i] - nums[j]) encountered so far
        int maxDiff = 0;
        // Store the maximum value encountered so far (potential nums[i])
        int preMax = 0;

        // Iterate through each number in the array
        for (int x : nums) {
            // Calculate potential answer using current number as nums[k]
            ans = Math.max(ans, (long)maxDiff * x);
            // Update maxDiff: current number could be nums[j]
            maxDiff = Math.max(maxDiff, preMax - x);
            // Update preMax: current number could be nums[i]
            preMax = Math.max(preMax, x);
        }

        return ans;
    }


    public static void main(String[] args) {
        // Test cases array with example inputs
        int[][] testCases = {
                {12, 6, 1, 2, 7},    // Expected output: 77  ( ( 12-1)*7)
                {1, 10, 3, 4, 19},   // Expected output: 133 ( ( 10-3)*19)
                {1, 2, 3}            // Expected output: 0   (no valid positive value possible)
        };

        // Run all test cases and print results
        for (int i = 0; i < testCases.length; i++) {
            long result = maximumTripletValue(testCases[i]);
            System.out.println("Test case " + (i + 1) + " result: " + result);
        }
    }
}

```





