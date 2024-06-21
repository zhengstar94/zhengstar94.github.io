---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Mastering Coin Change with Dynamic Programming"
date: "2023-09-01"
categories: 
  - "Data Structure"
---

# Mastering Coin Change with Dynamic Programming



This post will demonstrate the application of dynamic programming in solving change making problems. We will mainly discuss two aspects: finding out all combinations of coins that makeup a specific amount of money, and finding the minimum number of coins required to make up a specific amount of money. This technique is ideal for solving complex problems that need to be optimized.



## I. Finding Out All Combinations of Coins to Make Up a Specific Amount



The first problem we present is: suppose we have several different denominations of coins, we want to find out how many different ways we can make up a specific amount of money. This is a common problem in number theory and computer science and is applicable to many daily life scenarios, for instance, when you need to pay a certain amount of money, you would want to know how many ways that money can be paid.



We can use the following Java code for such scenarios:

```java
public int change(int amount, int[] coins) {
    // Initialize the dynamic programing array.
    int[] dp = new int[amount + 1];
    
    // There's only one way to make up the amount of 0, which is using 0 coins.
    dp[0] = 1; 
    
    // Iterating over the denomination of coins provided.
    for (int coin : coins) {
        
        // For each denomination, update the number of ways the amount can be made up.
        for (int x = coin; x < amount + 1; ++x) {
            dp[x] += dp[x - coin];
        }
    }
    
    // Return the number of ways to make up the amount using the provided denominations.
    return dp[amount];
}
```



The time complexity of this code is O(N*amount), where N is the number of coin denominations and `amount` is the target amount. The space complexity is O(amount), regardless of the number of coin types, only 'amount' states are needed to store all the necessary amounts.



## II. Finding Out the Minimum Number of Coins to Make Up a Specific Amount



The second problem is: How to make up a specific amount with the least number of coins. This is also a common problem in daily life. For example, when we go to shop at a supermarket, we want to pay a certain amount of money with the least number of coins. In this case, we need to consider how to choose coins.



For this problem, we can use the following Java code:

```java
public int coinChange(int[] coins, int amount) {
    // Initialize the maximum value.
    int max = amount + 1;  
    
    // Initialize the dynamic programing array, with all values set to max.
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, max);
    
    // Base case where the amount 0 can be achieved with no coins.
    dp[0] = 0;
    
    // Iterating over all amounts from 1 to the target amount.
    for (int i = 1; i <= amount; i++) {
        
        // For each denomination of the coin.
        for (int j = 0; j < coins.length; j++) {
            
            // If the denomination of the coin is not larger than the current amount.
            if (coins[j] <= i) {
                
                // Try to make up the current amount with fewer coins.
                dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
            }
        }
    }
    
    // The final value of dp[amount] is the minimum number of coins needed to make up the amount.
    return dp[amount] > amount ? -1 : dp[amount];
}
```



The time complexity of this code is also O(N*amount), where N is the number of coin denominations, and 'amount' is the payment amount. The space complexity also equals O(amount), regardless of the number of coin types, only 'amount' states are needed to store all the necessary amounts.



In conclusion, these two problems are classic applications of the dynamic programming method. By constantly trying new possibilities, breaking down the problem into smaller subproblems, and saving the answers to these subproblems, we can avoid repeated computations. This concludes the graceful solution to change making problems via dynamic programming.

