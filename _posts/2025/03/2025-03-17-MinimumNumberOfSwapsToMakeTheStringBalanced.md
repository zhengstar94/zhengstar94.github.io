---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1963. Minimum Number of Swaps to Make the String Balanced"
date: "2025-03-17"
tags: Medium
categories:
  - "LeetCode Math"
---

- You are given a **0-indexed** string `s` of **even** length `n`. The string consists of **exactly** `n / 2` opening brackets `'['` and `n / 2` closing brackets `']'`.
- A string is called **balanced** if and only if:
  - It is the empty string, or
  - It can be written as `AB`, where both `A` and `B` are **balanced** strings, or
  - It can be written as `[C]`, where `C` is a **balanced** string.
- You may swap the brackets at **any** two indices **any** number of times.
- Return *the **minimum** number of swaps to make* `s` ***balanced***.

**Example 1**

```
Input: s = "][]["
Output: 1
Explanation: You can make the string balanced by swapping index 0 with index 3.
The resulting string is "[[]]".
```

**Example 2**

```
Input: s = "]]][[["
Output: 2
Explanation: You can do the following to make the string balanced:
- Swap index 0 with index 4. s = "[]][][".
- Swap index 1 with index 5. s = "[[][]]".
The resulting string is "[[][]]".
```

**Example 3**

```
Input: s = "[]"
Output: 0
Explanation: The string is already balanced.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/03/17
 */
public class MinimumNumberOfSwapsToMakeTheStringBalanced {

    public static int minSwaps(String s) {
        int c = 0;  // Counter to track unmatched brackets
        for (char b : s.toCharArray()) {
            if (b == '[' || c == 0) {
                c++;    // Increment for opening bracket or when counter is 0
            } else {
                c--;    // Decrement for closing bracket when counter > 0
            }
        }
        return c / 2;   // Return minimum swaps needed
    }


    public static void main(String[] args) {
        // Test Case 1: Requires 1 swap to balance
        System.out.println("Test Case 1 Result: " + minSwaps("][")); // Expected output: 1

        // Test Case 2: Requires 2 swaps to balance
        System.out.println("Test Case 2 Result: " + minSwaps("]]][[[")); // Expected output: 2

        // Test Case 3: Already balanced, requires 0 swaps
        System.out.println("Test Case 3 Result: " + minSwaps("[]")); // Expected output: 0
    }
}

```





