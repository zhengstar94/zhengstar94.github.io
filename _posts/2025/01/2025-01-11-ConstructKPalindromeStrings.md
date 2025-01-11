---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1400. Construct K Palindrome Strings"
date: "2025-01-11"
tags: Medium
categories:
  - "LeetCode String"
---


- Given a string `s` and an integer `k`, return `true` *if you can use all the characters in* `s` *to construct* `k` *palindrome strings or* `false` *otherwise*.

**Example 1**

```
Input: s = "annabelle", k = 2
Output: true
Explanation: You can construct two palindromes using all characters in s.
Some possible constructions "anna" + "elble", "anbna" + "elle", "anellena" + "b"
```

**Example 2**

```
Input: s = "leetcode", k = 3
Output: false
Explanation: It is impossible to construct 3 palindromes using all the characters of s.
```

**Example 3**

```
Input: s = "true", k = 4
Output: true
Explanation: The only possible solution is to put each character in a separate  string.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2025/01/11
 */
public class ConstructKPalindromeStrings {
    public static boolean canConstruct(String s, int k) {
        // If k is greater than the string length, it's impossible to construct
        if (k > s.length()) {
            return false;
        }

        // Count the frequency of each character
        int[] count = new int[26];
        for (char c : s.toCharArray()) {
            count[c - 'a']++;
        }

        // Count the number of characters that appear odd times
        int oddCount = 0;
        for (int num : count) {
            if (num % 2 != 0) {
                oddCount++;
            }
        }

        // If the number of characters appearing odd times is greater than k,
        // we cannot construct k palindrome strings
        // If k=1, we can have any number of odd frequency characters
        // If k>1, the number of odd frequency characters cannot exceed k
        return oddCount <= k;
    }

    public static void main(String[] args) {
        // Test case 1
        String s1 = "annabelle";
        int k1 = 2;
        System.out.println("Test case 1: " + canConstruct(s1, k1)); // Should output true

        // Test case 2
        String s2 = "leetcode";
        int k2 = 3;
        System.out.println("Test case 2: " + canConstruct(s2, k2)); // Should output false

        // Test case 3
        String s3 = "true";
        int k3 = 4;
        System.out.println("Test case 3: " + canConstruct(s3, k3)); // Should output true
    }
}
```





