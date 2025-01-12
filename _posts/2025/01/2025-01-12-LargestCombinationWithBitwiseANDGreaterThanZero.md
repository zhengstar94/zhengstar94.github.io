---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2275. Largest Combination With Bitwise AND Greater Than Zero"
date: "2025-01-12"
tags: Medium
categories:
  - "LeetCode Math"
---


- The **bitwise AND** of an array `nums` is the bitwise AND of all integers in `nums`.
  - For example, for `nums = [1, 5, 3]`, the bitwise AND is equal to `1 & 5 & 3 = 1`.
  - Also, for `nums = [7]`, the bitwise AND is `7`.
- You are given an array of positive integers `candidates`. Compute the **bitwise AND** for all possible **combinations** of elements in the `candidates` array.
- Return *the size of the **largest** combination of* `candidates` *with a bitwise AND **greater** than* `0`.

**Example 1**

```
Input: candidates = [16,17,71,62,12,24,14]
Output: 4
Explanation: The combination [16,17,62,24] has a bitwise AND of 16 & 17 & 62 & 24 = 16 > 0.
The size of the combination is 4.
It can be shown that no combination with a size greater than 4 has a bitwise AND greater than 0.
Note that more than one combination may have the largest size.
For example, the combination [62,12,24,14] has a bitwise AND of 62 & 12 & 24 & 14 = 8 > 0.
```

**Example 2**

```
Input: candidates = [8,8]
Output: 2
Explanation: The largest combination [8,8] has a bitwise AND of 8 & 8 = 8 > 0.
The size of the combination is 2, so we return 2.
```

## Method 1

```tex
【O(nlogU) time | O(logU) space】
```

```java
package Leetcode.Math;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/01/12
 */
public class LargestCombinationWithBitwiseANDGreaterThanZero {
    public static int largestCombination(int[] candidates) {
        // Create an array to store the count of 1s at each binary position (0-23)
        // Size is 24 because the constraint states candidates[i] <= 10^7 < 2^24
        int[] cnt = new int[24];

        // Iterate through each number in the candidates array
        // For each number, we'll count how many 1s appear at each binary position
        for (int x : candidates) {
            // Process each bit of the current number until it becomes 0
            // i represents the current bit position (0-based, from right to left)
            // The loop continues as long as there are 1s remaining in x
            for (int i = 0; x > 0; i++) {
                // x & 1 extracts the rightmost bit (0 or 1)
                // Add this bit to the count at position i
                cnt[i] += x & 1;

                // Right shift x by 1 to process the next bit in the next iteration
                // This effectively removes the rightmost bit we just processed
                x >>= 1;
            }
        }

        // Find the maximum count across all bit positions
        // This represents the size of the largest possible combination
        // where all numbers have 1 at the same position
        return Arrays.stream(cnt).max().getAsInt();
    }

    public static void main(String[] args) {
        // Test case 1: Example with multiple valid combinations
        int[] candidates1 = {16,17,71,62,12,24,14};
        System.out.println("Test case 1 result: " + largestCombination(candidates1)); // Expected output: 4

        // Test case 2: Example with identical numbers
        int[] candidates2 = {8,8};
        System.out.println("Test case 2 result: " + largestCombination(candidates2)); // Expected output: 2

        // Test case 3: Example with large numbers using bit shifting
        int[] candidates3 = {1<<20, 1<<20, 1<<19, 1<<21};
        System.out.println("Test case 3 result: " + largestCombination(candidates3)); // Expected output: 2
    }
}

```





