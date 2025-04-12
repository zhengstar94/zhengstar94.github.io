---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2110. Number of Smooth Descent Periods of a Stock"
date: "2025-04-12"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---

- You are given an integer array `prices` representing the daily price history of a stock, where `prices[i]` is the stock price on the `ith` day.
- A **smooth descent period** of a stock consists of **one or more contiguous** days such that the price on each day is **lower** than the price on the **preceding day** by **exactly** `1`. The first day of the period is exempted from this rule.
- Return *the number of **smooth descent periods***.

**Example 1**

```
Input: prices = [3,2,1,4]
Output: 7
Explanation: There are 7 smooth descent periods:
[3], [2], [1], [4], [3,2], [2,1], and [3,2,1]
Note that a period with one day is a smooth descent period by the definition.
```

**Example 2**

```
Input: prices = [8,6,7,7]
Output: 4
Explanation: There are 4 smooth descent periods: [8], [6], [7], and [7]
Note that [8,6] is not a smooth descent period as 8 - 6 ≠ 1.
```

**Example 3**

```
Input: prices = [1]
Output: 1
Explanation: There is 1 smooth descent period: [1]
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/12
 */
public class NumberOfSmoothDescentPeriodsOfAStock {

    public static long getDescentPeriods(int[] prices) {
        // Handle edge cases: if array is null or empty
        if (prices == null || prices.length == 0){
            return 0;
        }

        // Initialize variables
        int n = prices.length;          // Length of the price array
        long result = 0;                // Total count of smooth descent periods
        int i = 0;                      // Current index in the array

        // Process the array using grouped loop pattern
        while (i < n){
            // Mark the start of current smooth descent sequence
            int start = i;

            // Extend the current smooth descent sequence as far as possible
            // A smooth descent requires each price to be exactly 1 less than the previous price
            while (i + 1 < n && prices[i] - prices[i + 1] == 1) {
                i++;
            }

            // Calculate the length of current smooth descent sequence
            long length = i - start + 1;

            /**
             * Calculate number of smooth descent periods in current sequence using arithmetic sequence sum
             * For a sequence of length k, we can form:
             * - k sequences of length 1
             * - (k-1) sequences of length 2
             * - (k-2) sequences of length 3
             * ... and so on
             *
             * Total number of sequences = k + (k-1) + (k-2) + ... + 1
             * This is equivalent to k * (k+1) / 2
             *
             * Example: For sequence [3,2,1] (length = 3)
             * Length 1 sequences: [3], [2], [1]                    (count: 3)
             * Length 2 sequences: [3,2], [2,1]                     (count: 2)
             * Length 3 sequences: [3,2,1]                          (count: 1)
             * Total = 3 + 2 + 1 = 6 = 3 * (3+1) / 2
             */
            result += (length * (length + 1)) / 2;

            // Move to the next element after current sequence
            i++;
        }

        return result;
    }
    
    public static void main(String[] args) {
        // Test case 1: Demonstrates a perfect smooth descent sequence
        // [3,2,1,4] contains following smooth descent periods:
        // Single day: [3], [2], [1], [4]
        // Two days: [3,2], [2,1]
        // Three days: [3,2,1]
        int[] prices1 = {3, 2, 1, 4};
        System.out.println("Test case 1 result: " + getDescentPeriods(prices1));

        // Test case 2: Demonstrates non-smooth descent sequence
        // [8,6,7,7] only contains single-day periods as no consecutive days form smooth descent
        int[] prices2 = {8, 6, 7, 7};
        System.out.println("Test case 2 result: " + getDescentPeriods(prices2));

        // Test case 3: Demonstrates single element case
        // [1] contains only one smooth descent period
        int[] prices3 = {1};
        System.out.println("Test case 3 result: " + getDescentPeriods(prices3));
    }
}

```





