---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1143.Longest Common Subsequence"
date: "2024-10-05"
categories:
  - "LeetCode Dynamic Programming"
---

- Given two strings `text1` and `text2`, return *the length of their longest **common subsequence**.* If there is no **common subsequence**, return `0`.

- A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

  - For example, `"ace"` is a subsequence of `"abcde"`.

  A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1**


```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```

**Example 2**

```
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```

**Example 3**

```
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

## Method 1

```tex
【O(m * n) time | O(m * n) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * @author zhengstars
 * @date 2024/10/05
 */
public class LongestCommonSubsequence {
    public static int longestCommonSubsequence(String text1, String text2) {
        // Get the lengths of the input strings
        int m = text1.length();
        int n = text2.length();

        // Create a 2D array to store the lengths of LCS
        // The size is (m+1) x (n+1) to handle empty string cases
        int[][] dp = new int[m + 1][n + 1];

        // Iterate through both strings
        for (int i = 1; i <= m ; i++) {
            for (int j = 1; j <= n; j++) {
                // If the characters at the current positions match
                if(text1.charAt(i - 1) == text2.charAt(j - 1)){
                    // Increment the LCS length from the previous state
                    // dp[i-1][j-1] represents the LCS length without these characters
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    // If characters don't match, take the maximum of:
                    // 1. LCS without the current character of text1 (dp[i-1][j])
                    // 2. LCS without the current character of text2 (dp[i][j-1])
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        // The bottom-right cell contains the length of the LCS of the entire strings
        return dp[m][n];
    }

    public static void main(String[] args) {

        // Test Case 1: Different strings with a common subsequence
        String text1 = "abcde";
        String text2 = "ace";
        int result1 = longestCommonSubsequence(text1, text2);
        System.out.println("Test Case 1:");
        System.out.println("Output: " + result1);
        System.out.println("Expected: 3");
        System.out.println();

        // Test Case 2: Identical strings
        text1 = "abc";
        text2 = "abc";
        int result2 = longestCommonSubsequence(text1, text2);
        System.out.println("Test Case 2:");
        System.out.println("Output: " + result2);
        System.out.println("Expected: 3");
        System.out.println();

        // Test Case 3: Completely different strings
        text1 = "abc";
        text2 = "def";
        int result3 = longestCommonSubsequence(text1, text2);
        System.out.println("Test Case 3:");
        System.out.println("Output: " + result3);
        System.out.println("Expected: 0");
    }
}
```





