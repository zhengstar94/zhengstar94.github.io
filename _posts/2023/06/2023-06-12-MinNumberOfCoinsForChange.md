---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Min Number Of Coins For Change"
date: "2023-06-12"
categories:
  - "Algorithm Dynamic"
---

# Min Number Of Coins For Change [Medium]

- Given an array of positive integers representing coin denominations and a single non-negative integer n representing a target amount of money, write a function that returns the smallest number of coins needed to make change for (to sum up to) that target amount using the given coin denominations.

- Note that you have access to an unlimited amount of coins. In other words, if the denominations are [1, 5, 10], you have access to an unlimited amount of 1s, 5s, and 10s.

- If it's impossible to make change for the target amount, return -1.


**Sample Input**

```
n = 7
denoms = [1, 5, 10]
```

**Sample Output**

```
3 // 2x1 + 1x5
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Try building an array of the minimum number of coins needed to make change for all amounts between 0 and n inclusive. Note that no coins are needed to make change for 0: in order to make change for 0, you do not need to use any coins. </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>Build up the array mentioned in Hint #1 one coin denomination at a time. In other words, find the minimum number of coins needed to make change for all amounts between 0 and n with only one denomination, then with two, etc., until you use all denominations. </strong></i>
</details>

<br>

## Method 1

```tex
【O(nd)time∣O(n)space】
```

```tex
1.\ Where\ n\ is\ the\ target\ amount\ and\ d\ is\ the\ number\ of\ coins\\
```

```java
package Dynamic;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2023/12/21
 */
public class MinNumberOfCoinsForChange {

    /**
     * Define the method minNumberOfCoinsForChange, which takes an integer n and an array denoms as input.
     * @param n
     * @param denoms
     * @return
     */
    public static int minNumberOfCoinsForChange(int n, int[] denoms) {

        // Create an array called numCoins with size n+1
        int[] numCoins = new int[n + 1];
        // Fill the numCoins array with the maximum possible integer values -1.
        Arrays.fill(numCoins, Integer.MAX_VALUE - 1);
        // As no coins are required to make the sum 0, set numCoins[0] to 0.
        numCoins[0] = 0;  

        // Loop through the denoms array.
        for (int denom : denoms) {

            // Loop through from 0 to n (inclusive)
            for (int amount = 0; amount < numCoins.length; amount++) {

                // If the denomination is less than or equal to the amount
                if (denom <= amount) {
                    // Find the minimum number of coins required by comparing the current number of coins and the number of coins required for the amount-denom, then add 1.
                    numCoins[amount] = Math.min(numCoins[amount], 1 + numCoins[amount - denom]);
                }
            }
        }
        // If numCoins[n] is not equal to Integer.MAX_VALUE, return numCoins[n], otherwise return -1.
        return numCoins[n] != Integer.MAX_VALUE - 1? numCoins[n] : -1;
    }

    /**
     * Define the main method
     * @param args
     */
    public static void main(String[] args) {
        // Define the array denoms with values 2 and 5.
        int[] denoms = {2,5};
        // Define n as 6.
        int n = 6;  

        // Call the method minNumberOfCoinsForChange and store its return value in numWays.
        int numWays = minNumberOfCoinsForChange(n, denoms);
        // Print numWays. The Output should be 3.
        System.out.println(numWays);  
    }
}
```







