---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3138. Minimum Length of Anagram Concatenation"
date: "2024-12-20"
tags: Medium
categories:
  - "LeetCode MathGeometry"
---

- You are given a string `s`, which is known to be a concatenation of **anagrams** of some string `t`.
- Return the **minimum** possible length of the string `t`.
- An **anagram** is formed by rearranging the letters of a string. For example, "aab", "aba", and, "baa" are anagrams of "aab".


**Example 1**

```
Input: s = "abba"
Output: 2
Explanation:
One possible string t could be "ba".
```

**Example 2**

```
Input: s = "cdef"
Output: 4
Explanation:
One possible string t could be "cdef", notice that t can be equal to s.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.MathGeometry;

/**
 * @author zhengxingxing
 * @date 2024/12/20
 */
public class MinimumLengthOfAnagramConcatenation {
    public static int minAnagramLength(String s) {
        int[] count = new int[26];
        int n = s.length();

        // Count frequency of each character
        for (char c : s.toCharArray()) {
            count[c - 'a']++;
        }

        int minLen = n;
        // Try each possible divisor from n down to 1
        for (int i = n; i >= 1; i--) {
            // Skip if length n is not divisible by i
            // Why: If n/i is not an integer, we cannot split string s into equal parts of length i
            // Example: If s.length=6, we cannot split it into parts of length 4
            if (n % i != 0) {
                continue;
            }

            // Check if the frequency of each character can be evenly distributed
            // among the substrings of length i
            // This is crucial because:
            // 1. Each character's frequency must be divisible by i to form valid anagrams
            // 2. If any character's frequency is not divisible by i, we cannot form
            //    equal distribution in each substring of length i
            boolean possible = true;
            for (int freq : count) {
                // For each character frequency, check if it's divisible by current length i
                // Example: if a character appears 3 times and i=2, it's impossible because
                // we cannot distribute 3 characters evenly into substrings of length 2
                if (freq % i != 0) {
                    possible = false;
                    break;  // Early termination if any frequency check fails
                }
            }

            // If all frequency checks passed (possible = true):
            // 1. Current length i is a valid candidate for the original string t
            // 2. Update minLen to track the minimum valid length found so far
            // 3. We don't break here because we need to check all possibilities to find minimum
            if (possible) {
                minLen = i;
            }
        }
        return minLen;
    }

    public static void main(String[] args) {

        // Test case 1: Regular case with repeated pattern
        String test1 = "aaabbb";
        System.out.println("Test case 1: s = \"" + test1 + "\"");
        System.out.println("Expected: 1");
        System.out.println("Output: " + minAnagramLength(test1));
        System.out.println();

        // Test case 2: String is already an anagram
        String test2 = "abba";
        System.out.println("Test case 2: s = \"" + test2 + "\"");
        System.out.println("Expected: 2");
        System.out.println("Output: " + minAnagramLength(test2));
        System.out.println();

        // Test case 3: All characters are same
        String test3 = "aaaaaa";
        System.out.println("Test case 3: s = \"" + test3 + "\"");
        System.out.println("Expected: 1");
        System.out.println("Output: " + minAnagramLength(test3));
        System.out.println();

        // Test case 4: No repeating pattern
        String test4 = "cdef";
        System.out.println("Test case 4: s = \"" + test4 + "\"");
        System.out.println("Expected: 4");
        System.out.println("Output: " + minAnagramLength(test4));
        System.out.println();

        // Test case 5: Complex pattern
        String test5 = "abcabcabc";
        System.out.println("Test case 5: s = \"" + test5 + "\"");
        System.out.println("Expected: 3");
        System.out.println("Output: " + minAnagramLength(test5));
    }
}

```





