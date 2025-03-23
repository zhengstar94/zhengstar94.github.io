---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "392. Is Subsequence"
date: "2025-03-23"
tags: Easy TwoPointers
categories:
  - "LeetCode DoubleSeqSubsequencePointers"
---

- Given two strings `s` and `t`, return `true` *if* `s` *is a **subsequence** of* `t`*, or* `false` *otherwise*.
- A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

**Example 1**

```
Input: s = "abc", t = "ahbgdc"
Output: true
```

**Example 2**

```
Input: s = "axc", t = "ahbgdc"
Output: false
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqSubsequencePointers;

/**
 * @author zhengxingxing
 * @date 2025/03/23
 */
public class IsSubsequence {
    
    public static boolean isSubsequence(String s, String t) {
        // Handle null input cases - if either string is null, return false
        if(s == null || t == null){
            return false;
        }

        // Empty source string is always a subsequence
        if (s.length() == 0){
            return true;
        }

        // If source string is longer than target, it can't be a subsequence
        if (s.length() > t.length()){
            return false;
        }

        // Initialize two pointers: one for source string s and one for target string t
        int sIndex = 0;  // Pointer for string s
        int tIndex = 0;  // Pointer for string t

        // Traverse both strings using the two pointers
        while (sIndex < s.length() && tIndex < t.length()){
            // If characters match, move source pointer forward
            if (s.charAt(sIndex) == t.charAt(tIndex)){
                sIndex++;
            }
            // Always move target pointer forward
            tIndex++;
        }

        // If we've matched all characters in s, sIndex will equal s.length()
        return sIndex == s.length();
    }
    
    public static void main(String[] args) {
        // Test Case 1: Normal case with expected output true
        String s1 = "abc";
        String t1 = "ahbgdc";
        System.out.println("Test Case 1 Result: " + isSubsequence(s1, t1)); // Expected output: true

        // Test Case 2: Normal case with expected output false
        String s2 = "axc";
        String t2 = "ahbgdc";
        System.out.println("Test Case 2 Result: " + isSubsequence(s2, t2)); // Expected output: false

        // Test Case 3: Empty source string
        String s3 = "";
        String t3 = "ahbgdc";
        System.out.println("Empty String Test Result: " + isSubsequence(s3, t3)); // Expected output: true

        // Test Case 4: Empty target string
        String s4 = "abc";
        String t4 = "";
        System.out.println("Empty Target String Test Result: " + isSubsequence(s4, t4)); // Expected output: false
    }
}
```





