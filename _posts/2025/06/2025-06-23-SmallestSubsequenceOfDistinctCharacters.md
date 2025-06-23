---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1081. Smallest Subsequence of Distinct Characters"
date: "2025-06-23"
tags: Medium
categories:
  - "LeetCode Stack"
---


- Given a string `s`, return *the* *lexicographically smallest* *subsequence* *of* `s` *that contains all the distinct characters of* `s` *exactly once*.

**Example 1**

```
Input: s = "bcabc"
Output: "abc"
```

**Example 2**

```
Input: s = "cbacdcbc"
Output: "acdb"
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Stack;

import java.util.ArrayDeque;
import java.util.Deque;

/**
 * @Author zhengxingxing
 * @Date 2025/06/23
 */
public class SmallestSubsequenceOfDistinctCharacters {

    public static String smallestSubsequence(String s) {
        // Array to count the remaining occurrences of each character in the string.
        // We use 128 to cover all ASCII characters.
        int[] count = new int[128];

        // Boolean array to record whether a character is already in the stack (result).
        boolean[] visited = new boolean[128];

        // First pass: count the occurrences of each character in the string.
        for (char c : s.toCharArray()){
            count[c]++;
        }

        // Stack to build the result subsequence.
        Deque<Character> stack = new ArrayDeque<>();

        // Iterate through each character in the string.
        for (char c : s.toCharArray()) {
            // Decrement the count since we are now visiting this character.
            count[c]--;

            // If the character is already in the stack, skip it to ensure uniqueness.
            if (visited[c]) {
                continue;
            }

            // This loop ensures the lexicographical order is minimal:
            // - While the stack is not empty,
            // - and the current character is smaller than the character at the top of the stack,
            // - and the character at the top of the stack will appear again later (count > 0),
            // we can safely remove the top character to get a smaller lexicographical order.
            while (!stack.isEmpty() && c < stack.peekLast() && count[stack.peekLast()] > 0) {
                // Mark the removed character as not visited so it can be added again later.
                visited[stack.pollLast()] = false;
            }

            // Add the current character to the stack and mark it as visited.
            stack.addLast(c);
            visited[c] = true;
        }

        // Build the final result from the stack.
        StringBuilder sb = new StringBuilder();
        for (char c : stack) {
            sb.append(c);
        }
        return sb.toString();
    }

    public static void main(String[] args) {
        String[] testCases = {"bcabc", "cbacdcbc"};
        for (String s : testCases) {
            System.out.println("Input: " + s);
            System.out.println("Output: " + smallestSubsequence(s));
        }
    }
}

```





