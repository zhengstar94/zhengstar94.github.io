---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3456. Find Special Substring of Length K"
date: "2025-04-10"
tags: Easy
categories:
  - "LeetCode GroupedLoop"
---


- You are given a string `s` and an integer `k`.
- Determine if there exists a substring of length **exactly** `k` in `s` that satisfies the following conditions:
  - The substring consists of **only one distinct character** (e.g., `"aaa"` or `"bbb"`).
  - If there is a character **immediately before** the substring, it must be different from the character in the substring.
  - If there is a character **immediately after** the substring, it must also be different from the character in the substring.
- Return `true` if such a substring exists. Otherwise, return `false`.

**Example 1**

```
Input: s = "aaabaaa", k = 3

Output: true

Explanation:

The substring s[4..6] == "aaa" satisfies the conditions.

It has a length of 3.
All characters are the same.
The character before "aaa" is 'b', which is different from 'a'.
There is no character after "aaa".
```

**Example 2**

```
Input: s = "abc", k = 2

Output: false

Explanation:

There is no substring of length 2 that consists of one distinct character and satisfies the conditions.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/10
 */
public class FindSpecialSubstringOfLengthK {

    public static boolean hasSpecialSubstring(String S, int k) {
        // Convert string to char array for easier manipulation
        char[] s = S.toCharArray();
        // Get the length of the string
        int n = s.length;
        // Initialize pointer for traversing the string
        int i = 0;

        // Outer loop: process each group of consecutive same characters
        while (i < n) {
            // Mark the start of current group
            int start = i;

            // Inner loop: find the end of current group of same characters
            while (i < n && s[i] == s[start]) {
                i++;
            }

            // Calculate the length of current group
            int groupLength = i - start;

            // Check if current group length matches required length k
            if (groupLength == k) {
                // Check character before the group (if exists)
                // Skip if previous character is same as group character
                if (start > 0 && s[start - 1] == s[start]) {
                    continue;
                }

                // Check character after the group (if exists)
                // Skip if next character is same as group character
                if (i < n && s[i] == s[start]) {
                    continue;
                }

                // Found valid special substring
                return true;
            }
        }

        // No valid special substring found
        return false;
    }


    public static void main(String[] args) {
        
        String s1 = "aaabaaa";
        int k1 = 3;
        System.out.println("Test case 1:");
        System.out.println("Input: s = \"" + s1 + "\", k = " + k1);
        System.out.println("Expected output: true");
        System.out.println("Actual output: " + hasSpecialSubstring(s1, k1));
        System.out.println();
        
        String s2 = "abc";
        int k2 = 2;
        System.out.println("Test case 2:");
        System.out.println("Input: s = \"" + s2 + "\", k = " + k2);
        System.out.println("Expected output: false");
        System.out.println("Actual output: " + hasSpecialSubstring(s2, k2));
        System.out.println();
    }
}

```
