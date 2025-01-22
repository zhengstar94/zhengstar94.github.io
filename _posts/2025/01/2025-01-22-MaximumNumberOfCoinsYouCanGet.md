---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1561. Maximum Number of Coins You Can Get"
date: "2025-01-22"
tags: Medium
categories:
  - "LeetCode Math"
---



- There are `3n` piles of coins of varying size, you and your friends will take piles of coins as follows:

  - In each step, you will choose **any** `3` piles of coins (not necessarily consecutive.
  - Of your choice, Alice will pick the pile with the maximum number of coins.
  - You will pick the next pile with the maximum number of coins.
  - Your friend Bob will pick the last pile.
  - Repeat until there are no more piles of coins.

- Given an array of integers `piles` where `piles[i]` is the number of coins in the `i^th` pile.

  Return the maximum number of coins that you can have.

**Example 1**

```
Input: piles = [2,4,1,2,7,8]
Output: 9
Explanation: Choose the triplet (2, 7, 8), Alice Pick the pile with 8 coins, you the pile with 7 coins and Bob the last one.
Choose the triplet (1, 2, 4), Alice Pick the pile with 4 coins, you the pile with 2 coins and Bob the last one.
The maximum number of coins which you can have are: 7 + 2 = 9.
On the other hand if we choose this arrangement (1, 2, 8), (2, 4, 7) you only get 2 + 4 = 6 coins which is not optimal.
```

**Example 2**

```
Input: piles = [2,4,5]
Output: 4
```

**Example 3**

```
Input: piles = [9,8,7,6,5,1,2,3,4]
Output: 18
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.Math;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/01/22
 */
public class MaximumNumberOfCoinsYouCanGet {
    public static int maxCoins(int[] piles) {
        // Step 1: Sort the piles array in ascending order
        Arrays.sort(piles);

        int n = piles.length; // Store the number of piles
        int result = 0;       // Variable to store the total coins you can get

        // Step 2: Traverse the array in reverse to select the "second largest" piles for yourself
        // Start from the second last pile (since the largest pile is for Alice),
        // stop at index n/3 (as Bob takes n/3 smallest piles), and decrement by 2 after each round
        // (to skip Alice's and Bob's piles).
        for (int i = n - 2; i >= n / 3; i -= 2) {
            result += piles[i]; // Add the second largest pile to your total
        }

        return result; // Return the total coins you can get
    }

    public static void main(String[] args) {
        // Test case 1
        int[] piles1 = {2, 4, 1, 2, 7, 8};
        System.out.println("Result for Test Case 1: " + maxCoins(piles1)); // Expected Output: 9

        // Test case 2
        int[] piles2 = {2, 4, 5};
        System.out.println("Result for Test Case 2: " + maxCoins(piles2)); // Expected Output: 4

        // Test case 3
        int[] piles3 = {9, 8, 7, 6, 5, 1, 2, 3, 4};
        System.out.println("Result for Test Case 3: " + maxCoins(piles3)); // Expected Output: 18
    }
}

```





