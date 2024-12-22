---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "1387. Sort Integers by The Power Value"
date: "2024-12-22"
tags: Medium
categories:
  - "LeetCode DFS"
---

- The power of an integer `x` is defined as the number of steps needed to transform `x` into `1` using the following steps:
    - if `x` is even then `x = x / 2`
    - if `x` is odd then `x = 3 * x + 1`
- For example, the power of `x = 3` is `7` because `3` needs `7` steps to become `1` (`3 --> 10 --> 5 --> 16 --> 8 --> 4 --> 2 --> 1`).
- Given three integers `lo`, `hi` and `k`. The task is to sort all integers in the interval `[lo, hi]` by the power value in **ascending order**, if two or more integers have **the same** power value sort them by **ascending order**.
- Return the `kth` integer in the range `[lo, hi]` sorted by the power value.
- Notice that for any integer `x` `(lo <= x <= hi)` it is **guaranteed** that `x` will transform into `1` using these steps and that the power of `x` is will **fit** in a 32-bit signed integer.


**Example 1**

```
Input: lo = 12, hi = 15, k = 2
Output: 13
Explanation: The power of 12 is 9 (12 --> 6 --> 3 --> 10 --> 5 --> 16 --> 8 --> 4 --> 2 --> 1)
The power of 13 is 9
The power of 14 is 17
The power of 15 is 17
The interval sorted by the power value [12,13,14,15]. For k = 2 answer is the second element which is 13.
Notice that 12 and 13 have the same power value and we sorted them in ascending order. Same for 14 and 15.
```

**Example 2**

```
Input: lo = 7, hi = 11, k = 4
Output: 7
Explanation: The power array corresponding to the interval [7, 8, 9, 10, 11] is [16, 3, 19, 6, 14].
The interval sorted by power is [8, 10, 11, 7, 9].
The fourth number in the sorted array is 7.
```

## Method 1

```tex
【O(nlog(n)) time | O(n) space】
```

```java
package Leetcode.DFS;

import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2024/12/22
 */
public class SortIntegersByThePowerValue {
    // Static HashMap to cache computation results, can be reused between multiple test cases
    private static final Map<Integer, Integer> memo = new HashMap<>();

    public static int getKth(int lo, int hi, int k) {
        // Create Integer array ranging from lo to hi
        Integer[] nums = new Integer[hi - lo + 1];
        // Initialize array using Arrays.setAll, more elegant than loop
        Arrays.setAll(nums, i -> i + lo);

        // Sort array using custom comparator
        Arrays.sort(nums, (x, y) -> {
            // Get power values for both numbers
            int fx = dfs(x), fy = dfs(y);
            // If power values are different, sort by power value
            // If power values are same, sort by original number
            return fx != fy ? fx - fy : x - y;
        });

        // Return kth number (k is 1-based, so subtract 1)
        return nums[k - 1];
    }

    
    private static int dfs(int i) {
        // Base case: if number is 1, power value is 0
        if (i == 1) {
            return 0;
        }

        // If we've calculated this number before, return cached result
        if (memo.containsKey(i)) {
            return memo.get(i);
        }

        // Handle odd numbers: combine two steps into one
        // Traditional: i -> 3i+1 -> (3i+1)/2, needs two steps
        // Optimized: directly calculate (3i+1)/2, counted as two steps
        if (i % 2 == 1) {
            memo.put(i, dfs((i * 3 + 1) / 2) + 2);
        }
        // Handle even numbers: divide by 2, counted as one step
        else {
            memo.put(i, dfs(i / 2) + 1);
        }

        // Return calculated power value
        return memo.get(i);
    }

    // Main method with test cases
    public static void main(String[] args) {

        // Test Case 1
        System.out.println("Test Case 1:");
        int result1 = getKth(12, 15, 2);
        System.out.println("Input: lo = 12, hi = 15, k = 2");
        System.out.println("Expected Output: 13");
        System.out.println("Actual Output: " + result1);
        System.out.println("Result: " + (result1 == 13 ? "PASS" : "FAIL"));
        System.out.println();

        // Test Case 2
        System.out.println("Test Case 2:");
        int result2 = getKth(7, 11, 4);
        System.out.println("Input: lo = 7, hi = 11, k = 4");
        System.out.println("Expected Output: 7");
        System.out.println("Actual Output: " + result2);
        System.out.println("Result: " + (result2 == 7 ? "PASS" : "FAIL"));
        System.out.println();

        // Test Case 3 (Additional test case)
        System.out.println("Test Case 3:");
        int result3 = getKth(1, 1, 1);
        System.out.println("Input: lo = 1, hi = 1, k = 1");
        System.out.println("Expected Output: 1");
        System.out.println("Actual Output: " + result3);
        System.out.println("Result: " + (result3 == 1 ? "PASS" : "FAIL"));
        System.out.println();

        // Test Case 4 (Additional test case with larger numbers)
        System.out.println("Test Case 4:");
        int result4 = getKth(1, 20, 10);
        System.out.println("Input: lo = 1, hi = 20, k = 10");
        System.out.println("Actual Output: " + result4);
        System.out.println("This test case demonstrates sorting for a larger range");
    }
}

```





