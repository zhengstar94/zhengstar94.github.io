---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3042. Count Prefix and Suffix Pairs I"
date: "2025-01-08"
tags: Easy
categories:
  - "LeetCode Array"
---


- You are given a **0-indexed** string array `words`.
- Let's define a **boolean** function `isPrefixAndSuffix` that takes two strings, `str1` and `str2`:
  - isPrefixAndSuffix(str1, str2) returns true if str1 is both a prefix and a suffix of str2, and false otherwise.
- For example, `isPrefixAndSuffix("aba", "ababa")` is `true` because `"aba"` is a prefix of `"ababa"` and also a suffix, but `isPrefixAndSuffix("abc", "abcd")` is `false`.
- Return *an integer denoting the **number** of index pairs* `(i, j)` *such that* `i < j`*, and* `isPrefixAndSuffix(words[i], words[j])` *is* `true`*.*

**Example 1**

```
Input: words = ["a","aba","ababa","aa"]
Output: 4
Explanation: In this example, the counted index pairs are:
i = 0 and j = 1 because isPrefixAndSuffix("a", "aba") is true.
i = 0 and j = 2 because isPrefixAndSuffix("a", "ababa") is true.
i = 0 and j = 3 because isPrefixAndSuffix("a", "aa") is true.
i = 1 and j = 2 because isPrefixAndSuffix("aba", "ababa") is true.
Therefore, the answer is 4.
```

**Example 2**

```
Input: words = ["pa","papa","ma","mama"]
Output: 2
Explanation: In this example, the counted index pairs are:
i = 0 and j = 1 because isPrefixAndSuffix("pa", "papa") is true.
i = 2 and j = 3 because isPrefixAndSuffix("ma", "mama") is true.
Therefore, the answer is 2.  
```

**Example 3**

```
Input: words = ["abab","ab"]
Output: 0
Explanation: In this example, the only valid index pair is i = 0 and j = 1, and isPrefixAndSuffix("abab", "ab") is false.
Therefore, the answer is 0.
```

## Method 1

```tex
【O(n^2 * m) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/01/08
 */
public class CountPrefixAndSuffixPairsI {
    public static int countPrefixSuffixPairs(String[] words) {
        // Counter for valid prefix-suffix pairs
        int count = 0;
        // Get array length to avoid repeated length calls
        int n = words.length;

        // Outer loop - iterate through potential prefix strings
        for (int i = 0; i < n - 1; i++) {
            // Inner loop - compare with all subsequent strings
            // Start from i+1 to ensure i < j condition
            for (int j = i + 1; j < n; j++) {
                // Check if words[i] is both prefix and suffix of words[j]
                if(isPrefixAndSuffix(words[i], words[j])){
                    count++;
                }
            }
        }

        return count;
    }

    private static boolean isPrefixAndSuffix(String str1, String str2) {
        // Get lengths of both strings for comparison
        int len1 = str1.length();
        int len2 = str2.length();

        // If str1 is longer than str2, it can't be a prefix or suffix
        if (len1 > len2){
            return false;
        }

        // Check if str1 is both a prefix and suffix of str2
        // using Java's built-in string methods
        return str2.startsWith(str1) && str2.endsWith(str1);
    }

    public static void main(String[] args) {
        // Test case 1
        String[] test1 = {"a", "aba", "ababa", "aa"};
        System.out.println("Test case 1 result: " + countPrefixSuffixPairs(test1)); // Expected output: 4

        // Test case 2
        String[] test2 = {"pa", "papa", "ma", "mama"};
        System.out.println("Test case 2 result: " + countPrefixSuffixPairs(test2)); // Expected output: 2

        // Test case 3
        String[] test3 = {"abab", "ab"};
        System.out.println("Test case 3 result: " + countPrefixSuffixPairs(test3)); // Expected output: 0
    }


}

```





