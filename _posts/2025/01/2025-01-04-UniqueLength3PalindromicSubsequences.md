---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1930. Unique Length-3 Palindromic Subsequences"
date: "2025-01-04"
tags: Medium
categories:
  - "LeetCode Two Pointers"
---


- Given a string `s`, return *the number of **unique palindromes of length three** that are a **subsequence** of* `s`.
- Note that even if there are multiple ways to obtain the same subsequence, it is still only counted **once**.
- A **palindrome** is a string that reads the same forwards and backwards.
- A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.
  - For example, `"ace"` is a subsequence of `"abcde"`.

**Example 1**

```
Input: s = "aabca"
Output: 3
Explanation: The 3 palindromic subsequences of length 3 are:
- "aba" (subsequence of "aabca")
- "aaa" (subsequence of "aabca")
- "aca" (subsequence of "aabca")
```

**Example 2**

```
Input: s = "adc"
Output: 0
Explanation: There are no palindromic subsequences of length 3 in "adc".
```

**Example 3**

```
Input: s = "bbcbaba"
Output: 4
Explanation: The 4 palindromic subsequences of length 3 are:
- "bbb" (subsequence of "bbcbaba")
- "bcb" (subsequence of "bbcbaba")
- "bab" (subsequence of "bbcbaba")
- "aba" (subsequence of "bbcbaba")
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2025/01/04
 */
public class UniqueLength3PalindromicSubsequences {

    public static int countPalindromicSubsequence(String s) {
        // Counter for unique palindromic subsequences
        int num = 0;

        // Iterate through all lowercase letters (a-z)
        for (int i = 0; i < 26; i++) {
            // Array to count occurrences of characters between two same letters
            int[] arr = new int[26];

            // Get current character and find its leftmost and rightmost positions
            char index = (char)(i + 'a');
            int left = s.indexOf(index);
            int right = s.lastIndexOf(index);

            // Count occurrences of each character between left and right positions
            for (int j = left + 1; j < right; j++) {
                arr[s.charAt(j) - 'a'] ++;
            }

            // Count unique characters between left and right positions
            // Each unique character forms a new palindromic subsequence
            for (int j = 0; j < 26; j++) {
                if(arr[j] != 0){
                    num++;
                }
            }
        }
        return num;
    }

    public static void main(String[] args) {
        // Test case 1
        System.out.println("Test case 1: aabca");
        System.out.println("Expected result: 3");
        System.out.println("Actual result: " + countPalindromicSubsequence("aabca"));

        // Test case 2
        System.out.println("\nTest case 2: adc");
        System.out.println("Expected result: 0");
        System.out.println("Actual result: " + countPalindromicSubsequence("adc"));

        // Test case 3
        System.out.println("\nTest case 3: bbcbaba");
        System.out.println("Expected result: 4");
        System.out.println("Actual result: " + countPalindromicSubsequence("bbcbaba"));

        // Edge cases
        System.out.println("\nEdge case 1: aa");
        System.out.println("Expected result: 0");
        System.out.println("Actual result: " + countPalindromicSubsequence("aa"));

        System.out.println("\nEdge case 2: aaaa");
        System.out.println("Expected result: 1");
        System.out.println("Actual result: " + countPalindromicSubsequence("aaaa"));
    }
}
```





