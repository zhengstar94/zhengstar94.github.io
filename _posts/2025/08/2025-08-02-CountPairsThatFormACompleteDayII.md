---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3185. Count Pairs That Form a Complete Day II"
date: "2025-08-02"
tags: Medium
categories:
  - "LeetCode Array"
---



- Given an integer array `hours` representing times in **hours**, return an integer denoting the number of pairs `i`, `j` where `i < j` and `hours[i] + hours[j]` forms a **complete day**.
- A **complete day** is defined as a time duration that is an **exact** **multiple** of 24 hours.
- For example, 1 day is 24 hours, 2 days is 48 hours, 3 days is 72 hours, and so on.

**Example 1**

```
Input: hours = [12,12,30,24,24]

Output: 2

Explanation: The pairs of indices that form a complete day are (0, 1) and (3, 4).
```

**Example 2**

```
Input: hours = [72,48,24,3]

Output: 3

Explanation: The pairs of indices that form a complete day are (0, 1), (0, 2), and (1, 2).
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @Author zhengxingxing
 * @Date 2025/08/02
 */
public class CountPairsThatFormACompleteDayII {
    /**
     * Counts the number of index pairs (i, j) such that i < j and hours[i] + hours[j] is a multiple of 24.
     *
     * @param hours An array where each element represents a duration in hours.
     * @return The number of valid index pairs forming a complete day.
     */
    public static long countCompleteDayPairs(int[] hours) {
        final int H = 24; // The number of hours in a complete day
        long ans = 0;     // To store the final count of valid pairs
        int[] cnt = new int[H]; // cnt[r] stores how many times remainder r has appeared so far

        // Iterate through each hour in the input array
        for (int t : hours) {
            // Step 1: Calculate the remainder of the current hour when divided by 24
            int rem = t % H;

            // Step 2: Calculate the required remainder for a valid pair
            // If rem + pairRem == 24 or rem + pairRem == 0 (i.e., divisible by 24)
            // For rem == 0, pairRem should also be 0
            int pairRem = (H - rem) % H;

            // Step 3: Add the number of previously seen elements with the required remainder
            // This counts all valid pairs where the previous element's remainder is pairRem
            ans += cnt[pairRem];

            // Step 4: Increment the count for the current remainder
            // This is done after counting to ensure i < j (current element only pairs with previous ones)
            cnt[rem]++;
        }

        return ans;
    }

    public static void main(String[] args) {
        int[] hours1 = {12, 12, 30, 24, 24};
        int[] hours2 = {72, 48, 24, 3};
        // Expected output: 2
        System.out.println(countCompleteDayPairs(hours1));
        // Expected output: 3
        System.out.println(countCompleteDayPairs(hours2));
    }
}

```





