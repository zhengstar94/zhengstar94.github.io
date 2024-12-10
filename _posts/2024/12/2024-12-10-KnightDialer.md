---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "935. Knight Dialer"
date: "2024-12-10"
tags: Medium
categories:
  - "LeetCode Dynamic Programming"
---

- The chess knight has a **unique movement**, it may move two squares vertically and one square horizontally, or two squares horizontally and one square vertically (with both forming the shape of an **L**). The possible movements of chess knight are shown in this diagram:
- Given an integer `n`, return how many distinct phone numbers of length `n` we can dial.
- You are allowed to place the knight **on any numeric cell** initially and then you should perform `n - 1` jumps to dial a number of length `n`. All jumps should be **valid** knight jumps.
- As the answer may be very large, **return the answer modulo** `109 + 7`.

**Example 1**

```
Input: n = 1
Output: 10
Explanation: We need to dial a number of length 1, so placing the knight over any numeric cell of the 10 cells is sufficient.
```

**Example 2**

```
Input: n = 2
Output: 20
Explanation: All the valid number we can dial are [04, 06, 16, 18, 27, 29, 34, 38, 40, 43, 49, 60, 61, 67, 72, 76, 81, 83, 92, 94]
```

**Example 3**

```
Input: n = 3131
Output: 136006598
Explanation: Please take care of the mod.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.DynamicProgramming;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2024/12/10
 */
public class KnightDialer {
    public static int knightDialer(int n) {
        // Special case: if number length is 1, return 10 (all digits 0-9 are valid)
        if(n == 1){
            return 10;
        }

        // Predefined array showing how many moves are possible from each digit
        // Index represents the current digit (0-9)
        // Value represents the number of possible next moves
        long[] help = {2,2,2,2,3,0,3,2,2,2};

        // Create a copy of the help array to track current state
        long[] cur = Arrays.copyOf(help, help.length);

        // Dynamic Programming: Iterate to build phone numbers of length n
        for (int i = 2; i < n; i++) {
            // Calculate possible moves for each digit based on previous state
            // These calculations follow the knight's move rules on a phone keypad
            cur[0] = help[4] + help[6];       // 0 can be reached from 4 and 6
            cur[1] = help[6] + help[8];        // 1 can be reached from 6 and 8
            cur[2] = help[7] + help[9];        // 2 can be reached from 7 and 9
            cur[3] = help[4] + help[8];        // 3 can be reached from 4 and 8
            cur[4] = help[3] + help[9] + help[0]; // 4 can be reached from 3, 9, and 0
            cur[5] = 0;                        // 5 cannot be reached by knight's move
            cur[6] = help[1] + help[7] + help[0]; // 6 can be reached from 1, 7, and 0
            cur[7] = help[2] + help[6];        // 7 can be reached from 2 and 6
            cur[8] = help[1] + help[3];        // 8 can be reached from 1 and 3
            cur[9] = help[2] + help[4];        // 9 can be reached from 2 and 4

            // Update help array and apply modulo to prevent integer overflow
            for (int j = 0; j < 10; j++){
                help[j] = cur[j] % (1000000007);
            }
        }

        // Sum up all possible phone numbers and apply final modulo
        long res = 0;
        for (int i = 0; i < 10; i++){
            res += cur[i];
            res = res % (1000000007);
        }
        return (int)res;
    }

    public static void main(String[] args) {
        // Test cases with different number lengths
        int n1 = 1;
        int result1 = knightDialer(n1);
        System.out.println("For n = " + n1 + ", the number of distinct phone numbers is: " + result1);

        int n2 = 2;
        int result2 = knightDialer(n2);
        System.out.println("For n = " + n2 + ", the number of distinct phone numbers is: " + result2);

        int n3 = 3131;
        int result3 = knightDialer(n3);
        System.out.println("For n = " + n3 + ", the number of distinct phone numbers is: " + result3);
    }
}

```





