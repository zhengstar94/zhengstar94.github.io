---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2412. Minimum Money Required Before Transactions"
date: "2025-01-25"
tags: Hard
categories:
  - "LeetCode Greedy"
---


- You are given a **0-indexed** 2D integer array `transactions`, where `transactions[i] = [costi, cashbacki]`.
- The array describes transactions, where each transaction must be completed exactly once in **some order**. At any given moment, you have a certain amount of `money`. In order to complete transaction `i`, `money >= costi` must hold true. After performing a transaction, `money` becomes `money - costi + cashbacki`.
- Return *the minimum amount of* `money` *required before any transaction so that all of the transactions can be completed **regardless of the order** of the transactions.*

**Example 1**

```
Input: transactions = [ [ 2,1],[5,0],[4,2 ] ]
Output: 10
Explanation:
Starting with money = 10, the transactions can be performed in any order.
It can be shown that starting with money < 10 will fail to complete all transactions in some order.
```

**Example 2**

```
Input: transactions = [ [ 3,0],[0,3 ] ]
Output: 3
Explanation:
- If transactions are in the order [ [ 3,0],[0,3 ] ], the minimum money required to complete the transactions is 3.
- If transactions are in the order [ [ 0,3],[3,0 ] ], the minimum money required to complete the transactions is 0.
Thus, starting with money = 3, the transactions can be performed in any order.

```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Greedy;

/**
 * @author zhengxingxing
 * @date 2025/01/25
 */
public class MinimumMoneyRequiredBeforeTransactions {

    /**
     * Calculate the minimum initial money required to complete all transactions
     *
     * @param transactions 2D array where each inner array contains [cost, cashback]
     * @return long value representing the minimum initial money needed
     *
     * Algorithm explanation:
     * 1. totalLose: Accumulates the total money lost from all losing transactions
     * 2. mx: Tracks the maximum of minimum starting money needed for any single transaction
     * 3. Final result = totalLose + mx
     */
    public static long minimumMoney(int[][] transactions) {
        // Track the total money lost from all losing transactions
        long totalLose = 0;
        // Track the maximum of minimum starting money needed for any transaction
        int mx = 0;

        // Iterate through each transaction
        for (int[] transaction : transactions) {
            // Calculate and accumulate losses
            // If cost > cashback, add the difference to totalLose
            // If cost <= cashback, add 0 (no loss)
            totalLose += Math.max(transaction[0] - transaction[1], 0);

            // Calculate minimum starting money needed for this transaction
            // Why min?: Because we need at least the smaller of (cost, cashback)
            // to start this transaction at any point
            int minStart = Math.min(transaction[0], transaction[1]);

            // Update mx if current transaction needs more starting money
            // This ensures we have enough money to start the most demanding transaction
            mx = Math.max(mx, minStart);
        }

        // Return total losses plus maximum starting money needed
        // This sum ensures we can complete all transactions in any order
        return totalLose + mx;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Mixed transactions with losses
        System.out.println("=== Example 1 ===");
        int[][] transactions1 = { { 2,1}, {5,0}, {4,2 } };
        long result1 = minimumMoney(transactions1);
        System.out.println("Minimum initial money needed for Example 1: " + result1);

        // Test Case 2: Transactions with extreme cashback differences
        System.out.println("\n\n=== Example 2 ===");
        int[][] transactions2 = { { 3,0}, {0,3 } };
        long result2 = minimumMoney(transactions2);
        System.out.println("Minimum initial money needed for Example 2: " + result2);

        // Test Case 3: More complex transactions with varying profits/losses
        System.out.println("\n\n=== Example 3 ===");
        int[][] transactions3 = { { 10,4}, {5,8}, {6,7 } };
        long result3 = minimumMoney(transactions3);
        System.out.println("Minimum initial money needed for Example 3: " + result3);
    }
}

```





