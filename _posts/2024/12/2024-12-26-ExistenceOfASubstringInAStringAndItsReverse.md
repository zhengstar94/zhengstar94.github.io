---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "3083. Existence of a Substring in a String and Its Reverse"
date: "2024-12-26"
tags: Easy
categories:
  - "LeetCode HashTable"
---


- Given a string `s`, find any substring of length `2` which is also present in the reverse of `s`.
- Return `true` *if such a substring exists, and* `false` *otherwise.*

**Example 1**

```
Input: s = "leetcode"

Output: true

Explanation: Substring "ee" is of length 2 which is also present  in reverse(s) == "edocteel".
```

**Example 2**

```
Input: s = "abcba"

Output: true

Explanation: All of the substrings of length 2 "ab", "bc", "cb", "ba" are also present  in reverse(s) == "abcba".
```

**Example 3**

```
Input: s = "abcd"

Output: false

Explanation: There is no substring of length 2 in s, which is also present  in the reverse of s.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.HashTable;

/**
 * @author zhengxingxing
 * @date 2024/12/26
 */
public class ExistenceOfASubstringInAStringAndItsReverse {
    
    public static boolean isSubstringPresent(String s) {
        // Get the length of input string
        int n = s.length();
        // If length is less than 2, no such substring exists
        if (n < 2) {
            return false;
        }

        // Create a 26x26 boolean array to store all possible two-character combinations
        boolean[][] seen = new boolean[26][26];

        // First pass: Mark all substrings of length 2 in the original string
        for (int i = 0; i < n - 1; i++) {
            int c1 = s.charAt(i) - 'a';
            int c2 = s.charAt(i + 1) - 'a';
            seen[c1][c2] = true;
        }

        // Second pass: Check if any reverse substring exists in the original string
        // Note: There was a bug in the original code where i++ should be i--
        for (int i = n - 1; i > 0; i--) {
            int c1 = s.charAt(i) - 'a';
            int c2 = s.charAt(i - 1) - 'a';
            if (seen[c1][c2]) {
                return true;
            }
        }

        return false;
    }
    
    public static void main(String[] args) {

        // Test Case 1
        String s1 = "leetcode";
        System.out.println("Test Case 1: " + s1);
        System.out.println("Expected: true");
        System.out.println("Result: " + isSubstringPresent(s1));
        System.out.println();

        // Test Case 2
        String s2 = "abcba";
        System.out.println("Test Case 2: " + s2);
        System.out.println("Expected: true");
        System.out.println("Result: " + isSubstringPresent(s2));
        System.out.println();

        // Test Case 3
        String s3 = "abcd";
        System.out.println("Test Case 3: " + s3);
        System.out.println("Expected: false");
        System.out.println("Result: " + isSubstringPresent(s3));
        System.out.println();

        // Additional Test Case 4: Empty string
        String s4 = "";
        System.out.println("Test Case 4: " + s4);
        System.out.println("Expected: false");
        System.out.println("Result: " + isSubstringPresent(s4));
    }
}
```





