---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "322.Coin Change"
date: "2024-09-23"
categories:
  - "LeetCode Dynamic Programming"
---

- You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.
- Return *the fewest number of coins that you need to make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `-1`.
- You may assume that you have an infinite number of each kind of coin.

**Example 1**

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

**Example 2**

```
Input: coins = [2], amount = 3
Output: -1
```

**Example 3**

```
Input: coins = [1], amount = 0
Output: 0
```

## Method 1

```tex
【O(amount * n) time | O(amount) space】
```

```java
package Leetcode.DynamicProgramming;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/09/23
 */
public class CoinChange {
    /**
     * Calculates the minimum number of coins needed to make the specified amount.
     *
     * @param coins An array of integers representing the coin denominations.
     * @param amount The target amount to be made using the coins.
     * @return The minimum number of coins needed to make the amount, or -1 if not possible.
     */
    public static int coinChange(int[] coins, int amount) {
        // Set a maximum value for comparison, which is greater than any possible number of coins.
        int max = amount + 1;

        // Create a dynamic programming array to store the minimum coins needed for each amount.
        int[] dp = new int[amount + 1];

        // Initialize the dp array with max values to signify that those amounts cannot be made initially.
        Arrays.fill(dp, max);

        // Base case: 0 coins are needed to make the amount of 0.
        dp[0] = 0;

        // Iterate through each amount from 1 to the target amount.
        for (int i = 1; i <= amount; i++) {
            // Check each coin denomination.
            for (int j = 0; j < coins.length; j++) {
                // Only proceed if the coin value is less than or equal to the current amount.
                if (coins[j] <= i) {
                    // Update the dp array at index i with the minimum value between the current value
                    // and the value at (i - coins[j]) + 1 (which represents using the current coin).
                    dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }

        // If dp[amount] is still greater than amount, it means it's not possible to form that amount.
        // Otherwise, return the minimum coins needed.
        return dp[amount] > amount ? -1 : dp[amount];
    }

    public static void main(String[] args) {

        // Test cases
        int[] coins1 = {1, 2, 5};
        int amount1 = 11;
        System.out.println("Minimum coins needed: " + coinChange(coins1, amount1)); // Output: 3

        int[] coins2 = {2};
        int amount2 = 3;
        System.out.println("Minimum coins needed: " + coinChange(coins2, amount2)); // Output: -1

        int[] coins3 = {1};
        int amount3 = 0;
        System.out.println("Minimum coins needed: " + coinChange(coins3, amount3)); // Output: 0

        int[] coins4 = {1, 2, 5};
        int amount4 = 10;
        System.out.println("Minimum coins needed: " + coinChange(coins4, amount4)); // Output: 2

        int[] coins5 = {1, 3, 4};
        int amount5 = 6;
        System.out.println("Minimum coins needed: " + coinChange(coins5, amount5)); // Output: 2
    }
}

```





