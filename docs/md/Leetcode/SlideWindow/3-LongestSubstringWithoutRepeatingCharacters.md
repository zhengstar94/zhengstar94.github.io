# LeetCode 3. Longest Substring Without Repeating Characters

- Given a string `s`, find the length of the **longest** **substring** without repeating characters.

**Example 1**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
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

