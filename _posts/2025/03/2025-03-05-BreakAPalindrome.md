---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1328. Break a Palindrome"
date: "2025-03-05"
tags: Medium
categories:
  - "LeetCode Greedy" 
---


- Given a palindromic string of lowercase English letters `palindrome`, replace **exactly one** character with any lowercase English letter so that the resulting string is **not** a palindrome and that it is the **lexicographically smallest** one possible.
- Return *the resulting string. If there is no way to replace a character to make it not a palindrome, return an **empty string**.*
- A string `a` is lexicographically smaller than a string `b` (of the same length) if in the first position where `a` and `b` differ, `a` has a character strictly smaller than the corresponding character in `b`. For example, `"abcc"` is lexicographically smaller than `"abcd"` because the first position they differ is at the fourth character, and `'c'` is smaller than `'d'`.

**Example 1**

```
Input: palindrome = "abccba"
Output: "aaccba"
Explanation: There are many ways to make "abccba" not a palindrome, such as "zbccba", "aaccba", and "abacba".
Of all the ways, "aaccba" is the lexicographically smallest.
```

**Example 2**

```
Input: palindrome = "a"
Output: ""
Explanation: There is no way to replace a single character to make "a" not a palindrome, so return an empty string.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Greedy;

/**
 * @author zhengxingxing
 * @date 2025/03/05
 */
public class BreakAPalindrome {
    public static String breakPalindrome(String palindrome) {
        // If string length is 1, impossible to break palindrome
        if (palindrome.length() <= 1){
            return "";
        }

        char[] chars = palindrome.toCharArray();
        int n = chars.length;

        // Check first half of string only due to palindrome property
        for (int i = 0; i < n / 2; i++) {
            // If character is not 'a', replace with 'a' for lexicographically smallest result
            if (chars[i] != 'a'){
                chars[i] = 'a';
                return new String(chars);
            }
        }

        // If all characters in first half are 'a', replace last character with 'b'
        chars[n - 1] = 'b';
        return new String(chars);
    }

    public static void main(String[] args) {
        // Test case 1: Regular palindrome
        String test1 = "abccba";
        System.out.println("Test case 1 input: " + test1);
        System.out.println("Test case 1 output: " + breakPalindrome(test1));

        // Test case 2: Single character
        String test2 = "a";
        System.out.println("Test case 2 input: " + test2);
        System.out.println("Test case 2 output: " + breakPalindrome(test2));

        // Test case 3: All 'a' characters
        String test3 = "aaaa";
        System.out.println("Test case 3 input: " + test3);
        System.out.println("Test case 3 output: " + breakPalindrome(test3));
    }
}

```





