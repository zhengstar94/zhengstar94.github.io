---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "5.Longest Palindromic Substring"
date: "2024-08-31"
categories:
  - "LeetCode Dynamic Programming"
---

# 5. Longest Palindromic Substring

- Given a string `s`, return *the longest* *palindromic* *substring* in `s`.

**Example 1**

```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

**Example 2**

```
Input: s = "cbbd"
Output: "bb"
```

## Method 1

```tex
【O(n^2) time | O(n^2) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * @author zhengstars
 * @date 2024/08/31
 */
public class LongestPalindromicSubstring {

    public static String longestPalindrome(String s) {
        // Get the length of the input string
        int n = s.length();

        // Create a 2D boolean array for dynamic programming
        // dp[i][j] will be true if the substring from index i to j is a palindrome
        boolean[][] dp = new boolean[n][n];

        // Variables to keep track of the longest palindrome found
        int start = 0;  // Starting index of the longest palindrome
        int maxLen = 1; // Length of the longest palindrome

        // Initialize base case: all substrings of length 1 are palindromes
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
        }

        // Fill the dp array
        // Outer loop: iterate over all possible lengths of substrings
        for (int len = 2; len <= n; len++) {
            // Inner loop: check all substrings of the current length
            for (int i = 0; i < n - len + 1; i++) {
                // Calculate the ending index of the current substring
                int j = i + len - 1;

                // Check if the current substring is a palindrome
                if (s.charAt(i) == s.charAt(j) && (len <= 3 || dp[i+1][j-1])) {
                    // Mark the current substring as a palindrome
                    dp[i][j] = true;

                    // Update the longest palindrome if necessary
                    if (len > maxLen) {
                        start = i;
                        maxLen = len;
                    }
                }
            }
        }

        // Return the longest palindromic substring
        return s.substring(start, start + maxLen);
    }


    // Main method for testing
    public static void main(String[] args) {
        // Test cases
        String[] testCases = {
                "babad",
                "cbbd",
                "a",
                "ac",
                "racecar",
                "aacabdkacaa",
                ""
        };

        // Expected results
        String[] expectedResults = {
                "bab",  // or "aba"
                "bb",
                "a",
                "a",
                "racecar",
                "aca",
                ""
        };

        // Run tests
        for (int i = 0; i < testCases.length - 1; i++) {
            String result = longestPalindrome(testCases[i]);
            System.out.println("Test Case " + (i + 1) + ":");
            System.out.println("Input: \"" + testCases[i] + "\"");
            System.out.println("Output: \"" + result + "\"");
            System.out.println("Expected: \"" + expectedResults[i] + "\"");
            System.out.println("Result: " + (result.equals(expectedResults[i]) ? "PASS" : "FAIL"));
            System.out.println();
        }
    }

}

```

