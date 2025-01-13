---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1208. Get Equal Substrings Within Budget"
date: "2025-01-13"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindow"
---


- You are given two strings `s` and `t` of the same length and an integer `maxCost`.
- You want to change `s` to `t`. Changing the `ith` character of `s` to `ith` character of `t` costs `|s[i] - t[i]|` (i.e., the absolute difference between the ASCII values of the characters).
- Return *the maximum length of a substring of* `s` *that can be changed to be the same as the corresponding substring of* `t` *with a cost less than or equal to* `maxCost`. If there is no substring from `s` that can be changed to its corresponding substring from `t`, return `0`.

**Example 1**

```
Input: s = "abcd", t = "bcdf", maxCost = 3
Output: 3
Explanation: "abc" of s can change to "bcd".
That costs 3, so the maximum length is 3.
```

**Example 2**

```
Input: s = "abcd", t = "cdef", maxCost = 3
Output: 1
Explanation: Each character in s costs 2 to change to character in t,  so the maximum length is 1.
```

**Example 3**

```
Input: s = "abcd", t = "acde", maxCost = 0
Output: 1
Explanation: You cannot make any change, so the maximum length is 1.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/13
 */
public class GetEqualSubstringsWithinBudget {
    public static int equalSubstring(String s, String t, int maxCost) {
        int n = s.length();
        // Array to store the conversion cost for each character
        int[] costs = new int[n];

        // Calculate the cost of changing each character from s to t
        for (int i = 0; i < n; i++) {
            costs[i] = Math.abs(s.charAt(i) - t.charAt(i));
        }

        int maxLength = 0;     // Track the maximum valid substring length
        int currentCost = 0;   // Track the current window's total cost
        int start = 0;         // Left pointer of the window

        // Iterate through the string with right pointer (end)
        for (int end = 0; end < n; end++) {
            // 1. Enter Window: Add cost of current character to window
            currentCost += costs[end];

            // 2. Exit Window: Shrink window while cost exceeds budget
            while (currentCost > maxCost) {
                currentCost -= costs[start];
                start++;
            }

            // 3. Update Answer: Update maximum length of valid substring
            maxLength = Math.max(maxLength, end - start + 1);
        }

        return maxLength;
    }

    public static void main(String[] args) {
        // Test Case 1: Multiple characters can be changed within budget
        String s1 = "abcd";
        String t1 = "bcdf";
        int maxCost1 = 3;
        System.out.println("Test case 1 result: " + equalSubstring(s1, t1, maxCost1)); // Expected output: 3

        // Test Case 2: High conversion cost limits substring length
        String s2 = "abcd";
        String t2 = "cdef";
        int maxCost2 = 3;
        System.out.println("Test case 2 result: " + equalSubstring(s2, t2, maxCost2)); // Expected output: 1

        // Test Case 3: Zero budget allows only identical characters
        String s3 = "abcd";
        String t3 = "acde";
        int maxCost3 = 0;
        System.out.println("Test case 3 result: " + equalSubstring(s3, t3, maxCost3)); // Expected output: 1
    }
}
```





