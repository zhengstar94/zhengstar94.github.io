---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "70.Climbing Stairs"
date: "2024-08-02"
categories:
  - "LeetCode Dynamic Programming"
---


# 70. Climbing Stairs

- You are climbing a staircase. It takes `n` steps to reach the top.
- Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example 1**

```
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

**Example 2**

```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * Solution for LeetCode problem: 70. Climbing Stairs
 * @author zhengstars
 * @date 2024/08/02
 */
public class ClimbingStairs {
    /**
     * Calculates the number of distinct ways to climb n stairs
     * @param n the number of stairs
     * @return the number of distinct ways to climb the stairs
     */
    public static int climbStairs(int n) {
        // Base case: if n is 0 or 1, there's only one way to climb
        if(n <= 1){
            return 1;
        }

        // Initialize variables for the first two steps
        int prev = 1, curr = 1;

        // Iterate from 2 to n, calculating the number of ways for each step
        for (int i = 2; i <= n; i++) {
            // Calculate the next number in the sequence
            int next = prev + curr;
            // Update prev and curr for the next iteration
            prev = curr;
            curr = next;
        }

        // Return the final result, which represents the number of ways to climb n stairs
        return curr;
    }

    /**
     * Main method for testing the climbStairs function
     */
    public static void main(String[] args) {
        // Test case 1: 2 stairs
        System.out.println("Number of ways to climb 2 stairs: " + climbStairs(2));

        // Test case 2: 3 stairs
        System.out.println("Number of ways to climb 3 stairs: " + climbStairs(3));
    }
}
```

