---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "(Review) 2944. Minimum Number of Coins for Fruits"
date: "2025-01-24"
tags: Medium Review
categories:
  - "LeetCode LeetCode Dynamic Programming"
---


- You are given an **1-indexed** integer array `prices` where `prices[i]` denotes the number of coins needed to purchase the `ith` fruit.
- The fruit market has the following reward for each fruit:
  - If you purchase the `ith` fruit at `prices[i]` coins, you can get any number of the next `i` fruits for free.
- **Note** that even if you **can** take fruit `j` for free, you can still purchase it for `prices[j]` coins to receive its reward.
- Return the **minimum** number of coins needed to acquire all the fruits.

**Example 1**

```
Input: prices = [3,1,2]
Output: 4

Explanation:
Purchase the 1st fruit with prices[0] = 3 coins, you are allowed to take the 2nd fruit for free.
Purchase the 2nd fruit with prices[1] = 1 coin, you are allowed to take the 3rd fruit for free.
Take the 3rd fruit for free.

Note that even though you could take the 2nd fruit for free as a reward of buying 1st fruit, you purchase it to receive its reward, which is more optimal.
```

**Example 2**

```
Input: prices = [1,10,1,1]
Output: 2

Explanation:
Purchase the 1st fruit with prices[0] = 1 coin, you are allowed to take the 2nd fruit for free.
Take the 2nd fruit for free.
Purchase the 3rd fruit for prices[2] = 1 coin, you are allowed to take the 4th fruit for free.
Take the 4th fruit for free.
```

**Example 3**

```
Input: prices = [26,18,6,12,49,7,45,45]
Output: 39

Explanation:
Purchase the 1st fruit with prices[0] = 26 coin, you are allowed to take the 2nd fruit for free.
Take the 2nd fruit for free.
Purchase the 3rd fruit for prices[2] = 6 coin, you are allowed to take the 4th, 5th and 6th (the next three) fruits for free.
Take the 4th fruit for free.
Take the 5th fruit for free.
Purchase the 6th fruit with prices[5] = 7 coin, you are allowed to take the 8th and 9th fruit for free.
Take the 7th fruit for free.
Take the 8th fruit for free.

Note that even though you could take the 6th fruit for free as a reward of buying 3rd fruit, you purchase it to receive its reward, which is more optimal.
```

## Method 1

```tex
【O(n^2) time | O(n) space】
```

```java
package Leetcode.DynamicProgramming;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/01/24
 */
public class MinimumNumberOfCoinsForFruits {
    public static int minimumCoins(int[] prices) {
        int n = prices.length; // Number of fruits
        if (n == 0) {
            return 0; // If there are no fruits, the cost is 0
        }

        // dp[i] represents the minimum cost to purchase the first (i+1) fruits
        int[] dp = new int[n];
        Arrays.fill(dp, Integer.MAX_VALUE); // Initialize all values to a large number
        dp[0] = prices[0]; // The first fruit must be purchased, so its cost is prices[0]

        // Iterate through each fruit starting from the second one
        for (int i = 1; i < n; i++) {
            // The starting point for j is i/2 because purchasing the j-th fruit
            // allows you to get the next j fruits for free
            int start = i / 2;

            // Iterate through all possible j values that can cover the i-th fruit
            for (int j = start; j <= i; j++) {
                // prev represents the cost to purchase the first j fruits
                int prev = (j > 0) ? dp[j - 1] : 0;

                // If prev is valid (not Integer.MAX_VALUE), update dp[i]
                if (prev != Integer.MAX_VALUE) {
                    dp[i] = Math.min(dp[i], prev + prices[j]);
                }
            }
        }

        // Return the minimum cost to purchase all fruits
        return dp[n - 1];
    }

    public static void main(String[] args) {
        // Test case 1
        System.out.println(minimumCoins(new int[]{3, 1, 2})); // Expected output: 4

        // Test case 2
        System.out.println(minimumCoins(new int[]{1, 10, 1, 1})); // Expected output: 2

        // Test case 3
        System.out.println(minimumCoins(new int[]{26, 18, 6, 12, 49, 7, 45, 45})); // Expected output: 39
    }
}

```





