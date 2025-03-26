---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2486. Append Characters to String to Make Subsequence"
date: "2025-03-26"
tags: Medium TwoPointers
categories:
  - "LeetCode DoubleSeqSubsequencePointers"
---


- You are given two strings `s` and `t` consisting of only lowercase English letters.
- Return *the minimum number of characters that need to be appended to the end of* `s` *so that* `t` *becomes a **subsequence** of* `s`.
- A **subsequence** is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

**Example 1**

```
Input: s = "coaching", t = "coding"
Output: 4
Explanation: Append the characters "ding" to the end of s so that s = "coachingding".
Now, t is a subsequence of s ("coachingding").
It can be shown that appending any 3 characters to the end of s will never make t a subsequence.
```

**Example 2**

```
Input: s = "abcde", t = "a"
Output: 0
Explanation: t is already a subsequence of s ("abcde").
```

**Example 3**

```
Input: s = "z", t = "abcde"
Output: 5
Explanation: Append the characters "abcde" to the end of s so that s = "zabcde".
Now, t is a subsequence of s ("zabcde").
It can be shown that appending any 4 characters to the end of s will never make t a subsequence.
```

## Method 1

```tex
【O(n + m) time | O(1) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqSubsequencePointers;

/**
 * @author zhengxingxing
 * @date 2025/03/26
 */
public class AppendCharactersToStringToMakeSubsequence {

    public static int appendCharacters(String s, String t) {
        // Initialize two pointers, i for string s and j for string t
        int i = 0, j = 0;

        // Loop until we reach the end of either string s or string t
        while (i < s.length() && j < t.length()) {
            // If the current characters of s and t match
            if (s.charAt(i) == t.charAt(j)) {
                // Move the pointer j to the next character in t
                j++;
            }
            // Always move the pointer i to the next character in s
            i++;
        }

        // The number of characters that need to be appended is the remaining characters in t
        // that have not been matched, which is t.length() - j
        return t.length() - j;
    }

    public static void main(String[] args) {
        // Test case 1
        String s1 = "coaching"; // Original string
        String t1 = "coding";   // Target string
        // Output the result of the appendCharacters method
        System.out.println("Example 1 Output: " + appendCharacters(s1, t1)); // Expected output: 4

        // Test case 2
        String s2 = "abcde"; // Original string
        String t2 = "a";     // Target string
        // Output the result of the appendCharacters method
        System.out.println("Example 2 Output: " + appendCharacters(s2, t2)); // Expected output: 0

        // Test case 3
        String s3 = "z";     // Original string
        String t3 = "abcde"; // Target string
        // Output the result of the appendCharacters method
        System.out.println("Example 3 Output: " + appendCharacters(s3, t3)); // Expected output: 5
    }
}
```





