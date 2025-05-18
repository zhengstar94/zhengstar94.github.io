---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3281. Maximize Score of Numbers in Ranges"
date: "2025-05-18"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.MaximizeMinimum"
---


- You are given an array of integers `start` and an integer `d`, representing `n` intervals `[start[i], start[i] + d]`.
- You are asked to choose `n` integers where the `ith` integer must belong to the `ith` interval. The **score** of the chosen integers is defined as the **minimum** absolute difference between any two integers that have been chosen.
- Return the **maximum** *possible score* of the chosen integers.

**Example 1**

```
Input: start = [6,0,3], d = 2

Output: 4

Explanation:

The maximum possible score can be obtained by choosing integers: 8, 0, and 4. The score of these chosen integers is min(|8 - 0|, |8 - 4|, |0 - 4|) which equals 4.
```

**Example 2**

```
Input: start = [2,6,13,13], d = 5

Output: 5

Explanation:

The maximum possible score can be obtained by choosing integers: 2, 7, 13, and 18. The score of these chosen integers is min(|2 - 7|, |2 - 13|, |2 - 18|, |7 - 13|, |7 - 18|, |13 - 18|) which equals 5.
```

## Method 1

```tex
【O(nlog(n)+nlog(A)) time | O(1) space】
```

```java
package Leetcode.BinarySearch.MaximizeMinimum;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/05/18
 */
public class MaximizeScoreOfNumbersInRanges {
    
    public static int maxPossibleScore(int[] start, int d) {
        // Sort the start array to ensure order for greedy assignment
        Arrays.sort(start);
        int n = start.length;

        /*
         * Calculate the upper bound for binary search 'right':
         * Explanation:
         * - The maximum possible difference between any two assigned numbers
         *   can be no more than the total length of the entire range divided evenly.
         * - The total range length is from the smallest start to the largest possible end:
         *   (start[n-1] + d) - start[0].
         * - Divide by (n-1) because there are (n-1) gaps between n numbers.
         * - Adding 1 ensures the upper bound is strictly greater,
         *   so the binary search range is safe.
         */
        int left = 0;
        int right = (start[n - 1] + d - start[0]) / (n - 1) + 1;

        // Binary search loop to find the maximum minimum difference
        while (left + 1 < right) {
            int mid = (left + right) >>> 1;  // mid = (left + right) / 2 without overflow

            /*
             * Check if it is possible to assign numbers with at least 'mid' difference
             * between adjacent assigned values while keeping each within allowed intervals.
             */
            if (check(start, d, mid)) {
                left = mid;   // If feasible, try a larger minimum difference
            } else {
                right = mid;  // Otherwise, try smaller differences
            }
        }

        // 'left' holds the largest minimum difference found
        return left;
    }
    
    private static boolean check(int[] start, int d, int score) {
        long prevPlacedValue = Long.MIN_VALUE;  // Initialize to very small to place first number freely

        for (int baseValue : start) {
            // Calculate the assigned value for current interval:
            // It must be at least 'score' away from the previously assigned value,
            // and also no less than the interval's start value.
            prevPlacedValue = Math.max(prevPlacedValue + score, baseValue);

            // If assigned value goes beyond the interval's max allowed value, fail
            if (prevPlacedValue > baseValue + d) {
                return false;
            }
        }

        // All intervals assigned successfully with at least 'score' difference
        return true;
    }

    public static void main(String[] args) {
        // Example 1:
        int[] start1 = {6, 0, 3};
        int d1 = 2;
        int result1 = maxPossibleScore(start1, d1);
        System.out.println("Example 1 output: " + result1); // Expected 4

        // Example 2:
        int[] start2 = {2, 6, 13, 13};
        int d2 = 5;
        int result2 = maxPossibleScore(start2, d2);
        System.out.println("Example 2 output: " + result2); // Expected 5
    }
}

```





