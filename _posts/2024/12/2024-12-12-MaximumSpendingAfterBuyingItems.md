---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2931. Maximum Spending After Buying Items"
date: "2024-12-12"
tags: Hard
categories:
  - "LeetCode Greedy"
---

- You are given a **0-indexed** `m * n` integer matrix `values`, representing the values of `m * n` different items in `m` different shops. Each shop has `n` items where the `jth` item in the `ith` shop has a value of `values[i][j]`. Additionally, the items in the `ith` shop are sorted in non-increasing order of value. That is, `values[i][j] >= values[i][j + 1]` for all `0 <= j < n - 1`.
- On each day, you would like to buy a single item from one of the shops. Specifically, On the `dth` day you can:
  - Pick any shop `i`.
  - Buy the rightmost available item `j` for the price of `values[i][j] * d`. That is, find the greatest index `j` such that item `j` was never bought before, and buy it for the price of `values[i][j] * d`.
- **Note** that all items are pairwise different. For example, if you have bought item `0` from shop `1`, you can still buy item `0` from any other shop.
- Return *the **maximum amount of money that can be spent** on buying all* `m * n` *products*.

**Example 1**

```
Input: values = [ [ 8,5,2],[6,4,1],[9,7,3 ] ]
Output: 285
Explanation: On the first day, we buy product 2 from shop 1 for a price of values[1][2] * 1 = 1.
On the second day, we buy product 2 from shop 0 for a price of values[0][2] * 2 = 4.
On the third day, we buy product 2 from shop 2 for a price of values[2][2] * 3 = 9.
On the fourth day, we buy product 1 from shop 1 for a price of values[1][1] * 4 = 16.
On the fifth day, we buy product 1 from shop 0 for a price of values[0][1] * 5 = 25.
On the sixth day, we buy product 0 from shop 1 for a price of values[1][0] * 6 = 36.
On the seventh day, we buy product 1 from shop 2 for a price of values[2][1] * 7 = 49.
On the eighth day, we buy product 0 from shop 0 for a price of values[0][0] * 8 = 64.
On the ninth day, we buy product 0 from shop 2 for a price of values[2][0] * 9 = 81.
Hence, our total spending is equal to 285.
It can be shown that 285 is the maximum amount of money that can be spent buying all m * n products. 
```

**Example 2**

```
Input: values = [ [ 10,8,6,4,2],[9,7,5,3,2 ] ]
Output: 386
Explanation: On the first day, we buy product 4 from shop 0 for a price of values[0][4] * 1 = 2.
On the second day, we buy product 4 from shop 1 for a price of values[1][4] * 2 = 4.
On the third day, we buy product 3 from shop 1 for a price of values[1][3] * 3 = 9.
On the fourth day, we buy product 3 from shop 0 for a price of values[0][3] * 4 = 16.
On the fifth day, we buy product 2 from shop 1 for a price of values[1][2] * 5 = 25.
On the sixth day, we buy product 2 from shop 0 for a price of values[0][2] * 6 = 36.
On the seventh day, we buy product 1 from shop 1 for a price of values[1][1] * 7 = 49.
On the eighth day, we buy product 1 from shop 0 for a price of values[0][1] * 8 = 64
On the ninth day, we buy product 0 from shop 1 for a price of values[1][0] * 9 = 81.
On the tenth day, we buy product 0 from shop 0 for a price of values[0][0] * 10 = 100.
Hence, our total spending is equal to 386.
It can be shown that 386 is the maximum amount of money that can be spent buying all m * n products.
```

## Method 1

```tex
【O(m * n * log(mn)) time | O(m * n) space】
```

```java
package Leetcode.Greedy;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2024/12/12
 */
public class MaximumSpendingAfterBuyingItems {
    public static long maxSpending(int[][] values) {
        // Get number of stores
        int m = values.length;

        // Get number of items per store
        int n = values[0].length;

        // Create a 1D array to store all item values
        // Total size will be number of stores * items per store
        int[] a = new int[m * n];

        // Copy all item values from 2D array to 1D array
        // System.arraycopy allows efficient array copying
        for (int i = 0; i < m; i++) {
            // Copy each store's items to the corresponding section of the 1D array
            // Source array: values[i]
            // Destination array: a
            // Destination start index: i * n (ensures non-overlapping placement)
            System.arraycopy(values[i], 0, a, i * n, n);
        }

        // Sort the array in ascending order
        // This ensures lower-value items are bought earlier
        // Higher-value items are bought later when day multiplier is larger
        Arrays.sort(a);

        // Variable to store total spending
        long ans = 0;

        // Calculate spending for each item
        // Multiply each item's value with its purchase day
        for (int i = 0; i < a.length; i++) {
            // (i + 1) represents the day of purchase
            // Lower-value items are multiplied by smaller day numbers
            // Higher-value items are multiplied by larger day numbers
            ans += (long)a[i] * (i + 1);
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test Case 1: Mixed value stores
        int[][] values1 = { { 8, 5, 2}, {6, 4, 1}, {9, 7, 3 } };
        long result1 = maxSpending(values1);
        System.out.println("Test Case 1 Result: " + result1);  // Expected output: 285

        // Test Case 2: More items per store
        int[][] values2 = { { 10, 8, 6, 4, 2}, {9, 7, 5, 3, 2 } };
        long result2 = maxSpending(values2);
        System.out.println("Test Case 2 Result: " + result2);  // Expected output: 386

        // Test Case 3: Single store
        int[][] values3 = { { 5, 3, 1 } };
        long result3 = maxSpending(values3);
        System.out.println("Test Case 3 Result: " + result3);

        // Test Case 4: All zero values
        int[][] values4 = { { 0, 0, 0}, {0, 0, 0 } };
        long result4 = maxSpending(values4);
    }
}

```





