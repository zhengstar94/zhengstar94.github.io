---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2781. Length of the Longest Valid Substring"
date: "2025-01-26"
tags: Hard
categories:
  - "LeetCode DynamicSlidingWindowMax"
---

- You are given a string `word` and an array of strings `forbidden`.
- A string is called **valid** if none of its substrings are present in `forbidden`.
- Return *the length of the **longest valid substring** of the string* `word`.
- A **substring** is a contiguous sequence of characters in a string, possibly empty.

**Example 1**

```
Input: word = "cbaaaabc", forbidden = ["aaa","cb"]
Output: 4
Explanation: There are 11 valid substrings in word: "c", "b", "a", "ba", "aa", "bc", "baa", "aab", "ab", "abc" and "aabc". The length of the longest valid substring is 4. 
It can be shown that all other substrings contain either "aaa" or "cb" as a substring. 
```

**Example 2**

```
Input: word = "leetcode", forbidden = ["de","le","e"]
Output: 4
Explanation: There are 11 valid substrings in word: "l", "t", "c", "o", "d", "tc", "co", "od", "tco", "cod", and "tcod". The length of the longest valid substring is 4.
It can be shown that all other substrings contain either "de", "le", or "e" as a substring. 
```

## Method 1

```tex
【O(L * nM^2) time | O(L) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

import java.util.Arrays;
import java.util.HashSet;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/01/26
 */
public class LengthOfTheLongestValidSubstring {
    public static int longestValidSubstring(String word, List<String> forbidden) {
        // Convert forbidden list to HashSet for O(1) lookup time
        HashSet<String> forbiddenSet = new HashSet<String>();
        forbiddenSet.addAll(forbidden);

        // Initialize variables
        int ans = 0;        // Store the maximum length of valid substring
        int left = 0;       // Left pointer of the sliding window
        int n = word.length(); // Length of input string

        // Outer loop: moves the right pointer of the sliding window
        for (int right = 0; right < n; right++) {
            // Inner loop: checks substrings within the current window [left, right]
            // This is a small sliding window check from right to left
            // Conditions:
            // 1. i >= left: ensures we don't check beyond the left boundary
            // 2. i > right - 10: optimizes by only checking up to 10 characters
            // (since forbidden strings are at most 10 characters long)
            for (int i = right; i >= left && i > right - 10; i--) {
                // Check if current substring [i, right] is in forbidden set
                // substring(i, right + 1) extracts the substring from index i to right (inclusive)
                if (forbiddenSet.contains(word.substring(i, right + 1))) {
                    // If found forbidden substring:
                    // 1. Update left pointer to skip the forbidden part
                    // 2. Move left pointer to i + 1 to ensure window starts after forbidden substring
                    left = i + 1;
                    // Break inner loop as we've found a forbidden substring
                    // No need to check shorter substrings as window has been adjusted
                    break;
                }
            }
            // After checking/adjusting window, update maximum length
            // right - left + 1 gives current window size
            ans = Math.max(ans, right - left + 1);
        }
        return ans;
    }

    public static void main(String[] args) {
        // Test case 1: Basic test with multiple forbidden strings
        String word1 = "cbaaaabc";
        List<String> forbidden1 = Arrays.asList("aaa", "cb");
        System.out.println("Test case 1 result: " + longestValidSubstring(word1, forbidden1)); // Expected: 4

        // Test case 2: Test with overlapping forbidden strings
        String word2 = "leetcode";
        List<String> forbidden2 = Arrays.asList("de", "le", "e");
        System.out.println("Test case 2 result: " + longestValidSubstring(word2, forbidden2)); // Expected: 4
    }
}

```





