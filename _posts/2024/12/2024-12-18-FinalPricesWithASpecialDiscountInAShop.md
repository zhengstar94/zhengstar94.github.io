---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1475. Final Prices With a Special Discount in a Shop"
date: "2024-12-18"
tags: Medium
categories:
  - "LeetCode Stack"
---

- You are given an integer array `prices` where `prices[i]` is the price of the `ith` item in a shop.
- There is a special discount for items in the shop. If you buy the `ith` item, then you will receive a discount equivalent to `prices[j]` where `j` is the minimum index such that `j > i` and `prices[j] <= prices[i]`. Otherwise, you will not receive any discount at all.
- Return an integer array `answer` where `answer[i]` is the final price you will pay for the `ith` item of the shop, considering the special discount.


**Example 1**

```
Input: prices = [8,4,6,2,3]
Output: [4,2,4,2,3]
Explanation: 
For item 0 with price[0]=8 you will receive a discount equivalent to prices[1]=4, therefore, the final price you will pay is 8 - 4 = 4.
For item 1 with price[1]=4 you will receive a discount equivalent to prices[3]=2, therefore, the final price you will pay is 4 - 2 = 2.
For item 2 with price[2]=6 you will receive a discount equivalent to prices[3]=2, therefore, the final price you will pay is 6 - 2 = 4.
For items 3 and 4 you will not receive any discount at all.
```

**Example 2**

```
Input: prices = [1,2,3,4,5]
Output: [1,2,3,4,5]
Explanation: In this case, for all items, you will not receive any discount at all.
```

**Example 3**

```
Input: prices = [10,1,1,6]
Output: [9,0,1,6]
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.ArrayDeque;
import java.util.Deque;

/**
 * @author zhengxingxing
 * @date 2024/12/18
 */
public class FinalPricesWithASpecialDiscountInAShop {
    public static int[] finalPrices(int[] prices) {
        // Get the length of the prices array
        int n = prices.length;
        // Create an answer array to store final prices
        int[] answer = new int[n];

        // Create a monotonic stack to track potential discount indices
        // The stack will store indices, not the actual prices
        Deque<Integer> stack = new ArrayDeque<>();

        // Iterate through all prices
        for (int i = 0; i < n; i++) {
            // While the stack is not empty and the price at the top of the stack 
            // is greater than or equal to the current price
            while (!stack.isEmpty() && prices[stack.peek()] >= prices[i]){
                // Pop the index from the stack
                // This means we've found a discount for the previous item
                int prevIndex = stack.pop();

                // Calculate the final price by subtracting the current price
                // This is the special discount rule
                answer[prevIndex] = prices[prevIndex] - prices[i];
            }

            // Push the current index onto the stack
            // This index might be a potential discount for future items
            stack.push(i);
        }

        // Process any remaining indices in the stack
        // These are items that did not receive any discount
        for (int index: stack) {
            // Restore the original price for items with no discount
            answer[index] = prices[index];
        }

        // Return the final prices array
        return answer;
    }

    public static void main(String[] args) {
        // Test case 1: Mixed prices with various discounts
        int[] prices1 = {8, 4, 6, 2, 3};
        int[] result1 = finalPrices(prices1);
        System.out.print("Test Case 1 Result: ");
        for (int price : result1) {
            System.out.print(price + " ");
        }
        System.out.println();

        // Test case 2: Prices in ascending order (no discounts)
        int[] prices2 = {1, 2, 3, 4, 5};
        int[] result2 = finalPrices(prices2);
        System.out.print("Test Case 2 Result: ");
        for (int price : result2) {
            System.out.print(price + " ");
        }
        System.out.println();

        // Test case 3: Another mixed scenario with discounts
        int[] prices3 = {10, 1, 1, 6};
        int[] result3 = finalPrices(prices3);
        System.out.print("Test Case 3 Result: ");
        for (int price : result3) {
            System.out.print(price + " ");
        }
    }
}
```





