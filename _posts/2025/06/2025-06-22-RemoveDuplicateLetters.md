---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "316. Remove Duplicate Letters"
date: "2025-06-22"
tags: Medium
categories:
  - "LeetCode Stack"
---


- Given a string `s`, remove duplicate letters so that every letter appears once and only once. You must make sure your result is **the smallest in lexicographical order** among all possible results.

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
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.ArrayDeque;
import java.util.Deque;

/**
 * @Author zhengxingxing
 * @Date 2025/06/22
 */
public class RemoveDuplicateLetters {

    public static String removeDuplicateLetters(String s) {
        int[] count = new int[26]; // Array to count the remaining occurrences of each character.
        boolean[] inStack = new boolean[26]; // Flags to indicate if a character is already in the stack.

        // First pass: count the occurrences of each character in the string.
        for (char c : s.toCharArray()) {
            count[c - 'a']++;
        }

        Deque<Character> stack = new ArrayDeque<>(); // Monotonic stack to build the result.

        // Iterate through each character in the string.
        for (char c : s.toCharArray()) {
            count[c - 'a']--; // Decrement the count for this character, as it's now being processed.

            // If the character is already in the stack, skip it to avoid duplicates.
            if (inStack[c - 'a']) {
                continue;
            }

            /**
             * While the stack is not empty, and the current character is lexicographically
             * smaller than the character at the top of the stack, and the character at the
             * top of the stack will appear again later (count > 0):
             *   - Pop the top character from the stack.
             *   - Mark it as not in the stack.
             * This ensures that:
             *   1. The result remains lexicographically smallest by removing bigger letters
             *      that can still be placed later.
             *   2. Each letter appears only once in the result.
             */
            while (!stack.isEmpty()
                    && c < stack.peekLast()
                    && count[stack.peekLast() - 'a'] > 0) {
                char removed = stack.removeLast(); // Remove the top character from the stack.
                inStack[removed - 'a'] = false;    // Mark the removed character as not in the stack.
            }

            // Add the current character to the stack and mark it as present.
            stack.addLast(c);
            inStack[c - 'a'] = true;
        }

        // Build the final result string from the characters in the stack.
        StringBuilder sb = new StringBuilder();
        for (char c : stack) {
            sb.append(c);
        }
        return sb.toString();
    }
    
    public static void main(String[] args) {
        System.out.println(removeDuplicateLetters("bcabc"));      // Output: "abc"
        System.out.println(removeDuplicateLetters("cbacdcbc"));   // Output: "acdb"
    }
}

```





