---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3330. Find the Original Typed String I"
date: "2025-07-01"
tags: Easy
categories:
  - "LeetCode String"
---

- Alice is attempting to type a specific string on her computer. However, she tends to be clumsy and **may** press a key for too long, resulting in a character being typed **multiple** times.
- Although Alice tried to focus on her typing, she is aware that she may still have done this **at most** *once*.
- You are given a string `word`, which represents the **final** output displayed on Alice's screen.
- Return the total number of *possible* original strings that Alice *might* have intended to type.

**Example 1**

```
Input: word = "abbcccc"

Output: 5

Explanation:

The possible strings are: "abbcccc", "abbccc", "abbcc", "abbc", and "abcccc".
```

**Example 2**

```
Input: word = "abcd"

Output: 1

Explanation:

The only possible string is "abcd".
```

**Example 3**

```
Input: word = "aaaa"

Output: 4
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @Author zhengxingxing
 * @Date 2025/07/01
 */
public class FindTheOriginalTypedStringI {

    public static int possibleStringCount(String word) {
        // Initialize the count to 1.
        // This is because the original string itself (with no long press at all) is always a valid possibility.
        int count = 1;

        // Start from the second character and iterate through the string.
        // We are looking for all pairs of adjacent, identical characters.
        // Each such pair represents a possible place where a "long press" could have occurred.
        for (int i = 1; i < word.length(); i++) {
            // If the current character is the same as the previous one,
            // it means there is a sequence of repeated characters.
            // Each such pair gives us one more possible original string,
            // because we could "remove" one of these repeated characters (simulating that the long press happened here).
            if (word.charAt(i) == word.charAt(i - 1)) {
                count++;
            }
        }

        // Return the total count.
        // This includes:
        //   - The original string itself (no long press)
        //   - One for each possible pair of adjacent identical characters (where a long press could have happened)
        return count;
    }

    public static void main(String[] args) {
        System.out.println(possibleStringCount("abbcccc")); // Output: 5

        System.out.println(possibleStringCount("abcd"));    // Output: 1

        System.out.println(possibleStringCount("aaaa"));    // Output: 4
    }
}

```





