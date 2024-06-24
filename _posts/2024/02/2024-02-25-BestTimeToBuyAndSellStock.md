---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "121. Best Time to Buy and Sell Stock"
date: "2024-02-25"
categories:
  - "LeetCode SlideWindow"
---

# LeetCode 121. Best Time to Buy and Sell Stock

- You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.
- You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.
- Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.

**Example 1**

```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```

**Example 2**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode;

/**
 * @author zhengstars
 * @date 2024/03/21
 */
public class BestTimeToBuyAndSellStock {
    public static int maxProfit(int[] prices) {
        // minprice is used to store the minimal price of the stock
        int minprice = Integer.MAX_VALUE;
        // maxprofit is used to store the maximum profit
        int maxprofit = 0;

        // This loop is used to traverse on every price
        for (int i = 0; i < prices.length; i++) {
            // If the current price is less than minprice, update minprice
            if (prices[i] < minprice) {
                minprice = prices[i];
            }
            // If the current profit (price - minprice) is greater than maxprofit, update maxprofit
            else if (prices[i] - minprice > maxprofit) {
                maxprofit = prices[i] - minprice;
            }
        }
        // Return the maximum profit
        return maxprofit;
    }

    public static void main(String[] args) {
        // Example 1: Given the stock prices of {7,1,5,3,6,4}
        int[] prices1 = {7,1,5,3,6,4};
        // Run the maxProfit function and print out the result.
        System.out.println(maxProfit(prices1));  // Output: 5

        // Example 2: Given the stock prices of {7,6,4,3,1}
        int[] prices2 = {7,6,4,3,1};
        // Run the maxProfit function and print out the result.
        System.out.println(maxProfit(prices2));  // Output: 0
    }
}

```

