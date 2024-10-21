---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "122. Best Time to Buy and Sell Stock II"
date: "2024-10-21"
categories:
  - "LeetCode SlideWindow"
---



- You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.
- On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.
- Find and return *the **maximum** profit you can achieve*.


**Example 1**

```
Input: prices = [ 7,1,5,3,6,4 ]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.
```

**Example 2**

```
Input: prices = [ 1,2,3,4,5 ]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.
```

**Example 3**

```
Input: prices = [ 7,6,4,3,1 ]
Output: 0
Explanation: There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengstars
 * @date 2024/03/21
 */
public class BestTimeToBuyAndSellStockII {
    public static int maxProfit(int[] prices) {
        // Edge case: If array is null or has less than 2 elements, no profit can be made
        if(prices == null || prices.length <= 1){
            return 0;
        }

        // Variable to keep track of total profit from all transactions
        int totalProfit = 0;

        // Iterate through the array starting from index 1 to compare with previous day
        for (int i = 1; i < prices.length; i++) {
            // If today's price is higher than yesterday's price
            // We can make a profit by buying yesterday and selling today
            if(prices[i] > prices[i -1]){
                // Add the price difference to our total profit
                // This is equivalent to buying at yesterday's price and selling at today's price
                totalProfit += prices[i] - prices[i -1];
            }
        }

        return totalProfit;
    }

    /**
     * Main method to test the solution with example cases
     */
    public static void main(String[] args) {

        int[] prices1 = { 7,1,5,3,6,4 };
        System.out.println(maxProfit(prices1));  // Output: 7

        int[] prices2 = { 7,6,4,3,1 };
        System.out.println(maxProfit(prices2));  // Output: 0
    }
}

```





