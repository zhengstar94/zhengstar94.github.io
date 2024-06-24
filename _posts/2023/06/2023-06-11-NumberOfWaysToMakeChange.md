---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Number Of Ways To Make Change"
date: "2023-06-11"
categories:
  - "Algorithm Dynamic"
---

# Number Of Ways To Make Change [Medium]

- Given an array of distinct positive integers representing coin denominations and a single non-negative integer n representing a target amount of money, write a function that returns the number of ways to make change for that target amount using the given coin denominations.
- Note that an unlimited amount of coins is at your disposal.

**Sample Input**

```
n = 6
denoms = [1, 5]
```

**Sample Output**

```
2 // 1x1 + 1x5 and 6x1
```

<br>

**Hints**
<br>

<details> <summary><b>Hint 1</b></summary>
    <br>
    <i><strong> Try building an array of the number of ways to make change for all amounts between 0 and n inclusive. Note that there is only one way to make change for 0: that is to not use any coins. </strong></i>
</details>

<br>

<details> <summary><b>Hint 2</b></summary>
    <br>
    <i><strong>Build up the array mentioned in Hint #1 one coin denomination at a time. In other words, find the number of ways to make change for all amounts between 0 and n with only one denomination, then with two, etc., until you use all denominations.</strong></i>
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

/**
 * @author zhengstars
 * @date 2023/12/18
 */
public class NumberOfWaysToMakeChange {

    public static int numberOfWaysToMakeChange(int n, int[] denoms) {
        // Initialize a ways array filled with 0's with a length of n + 1.
        int[] ways = new int[n + 1];

        // There is only one way to return 0 coins change.
        ways[0] = 1;

        // For each coin denomination...
        for (int denom : denoms) {
            // Try to use it to make up amounts up to n
            for (int amount = 1; amount < n + 1; amount++) {
                // If the current denom is less than or equal to the amount,
                // then there are ways[amount - denom] ways to make change.
                if (denom <= amount) {
                    ways[amount] += ways[amount - denom];
                }
            }
        }
        // After checking all denominations, return the number of ways to make an amount of n.
        return ways[n];
    }

    public static void main(String[] args) {
        int[] denoms = {1,2,5};
        int n = 10;

        int numWays = numberOfWaysToMakeChange(n, denoms);
        System.out.println(numWays);  // Output: 10
    }
}

```







