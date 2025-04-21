---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "467. Unique Substrings in Wraparound String"
date: "2025-04-21"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---

- We define the string `base` to be the infinite wraparound string of `"abcdefghijklmnopqrstuvwxyz"`, so `base` will look like this:
  - `"...zabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcd...."`.
- Given a string `s`, return *the number of **unique non-empty substrings** of* `s` *are present in* `base`.

**Example 1**

```
Input: s = "a"
Output: 1
Explanation: Only the substring "a" of s is in base.
```

**Example 2**

```
Input: s = "cac"
Output: 2
Explanation: There are two substrings ("a", "c") of s in base.
```

**Example 3**

```
Input: s = "zab"
Output: 6
Explanation: There are six substrings ("z", "a", "b", "za", "ab", and "zab") of s in base.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/21
 */
public class UniqueSubstringsInWraparoundString {

    public static int findSubstringInWraproundString(String s) {
        // dp[i] stores the maximum length of continuous substring ending with character (i+'a')
        int[] dp = new int[26];
        int n = s.length();
        int i = 0;

        // Outer loop: process the entire string
        while (i < n) {
            // Mark the start of current continuous group
            int start = i;

            // Inner loop: extend the current group as far as possible
            // A group continues if:
            // 1. Next character is one more than current (e.g., 'a' to 'b')
            // 2. Current is 'z' and next is 'a'
            while (i + 1 < n &&
                    (s.charAt(i + 1) - s.charAt(i) == 1 ||
                            (s.charAt(i) == 'z' && s.charAt(i + 1) == 'a'))) {
                i++;
            }

            // Calculate length of current continuous group
            int len = i - start + 1;

            // Process each character in the current group
            // For each character, update the maximum length of continuous substring ending with it
            for (int j = 0; j < len; j++) {
                // Calculate index in dp array (0 for 'a', 1 for 'b', etc.)
                int idx = s.charAt(start + j) - 'a';

                // len-j represents number of possible substrings ending at current character
                // Example: for "abc", at 'a':
                // - len=3, j=0: dp['a'] = max(dp['a'], 3) for substrings "a", "ab", "abc"
                // at 'b':
                // - len=3, j=1: dp['b'] = max(dp['b'], 2) for substrings "b", "bc"
                // at 'c':
                // - len=3, j=2: dp['c'] = max(dp['c'], 1) for substring "c"
                dp[idx] = Math.max(dp[idx], len - j);
            }

            // Move to next character
            i++;
        }

        // Calculate sum of all unique substring counts
        int sum = 0;
        for (int count : dp) {
            sum += count;
        }
        return sum;
    }

    public static void main(String[] args) {
        // Test Case 1: Single character
        String s1 = "a";
        System.out.println("Test Case 1 Result: " + findSubstringInWraproundString(s1)); // Expected: 1

        // Test Case 2: Non-continuous characters
        String s2 = "cac";
        System.out.println("Test Case 2 Result: " + findSubstringInWraproundString(s2)); // Expected: 2

        // Test Case 3: Continuous characters including wraparound
        String s3 = "zab";
        System.out.println("Test Case 3 Result: " + findSubstringInWraproundString(s3)); // Expected: 6
    }
}

```





