---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "132. Palindrome Partitioning II"
date: "2025-03-02"
tags: Hard
categories:
  - "LeetCode Dynamic Programming" 
---


- Given a string `s`, partition `s` such that every substring of the partition is a palindrome.
- Return *the **minimum** cuts needed for a palindrome partitioning of* `s`.

**Example 1**

```
Input: s = "aab"
Output: 1
Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
```

**Example 2**

```
Input: s = "a"
Output: 0
```

**Example 3**

```
Input: s = "ab"
Output: 1
```

## Method 1

```tex
【O(n²) time | O(n²) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * @author zhengxingxing
 * @date 2025/03/02
 */
public class PalindromePartitioningII {
    public static int minCut(String s) {
        // Base case: empty string or single character string needs 0 cuts
        if (s == null || s.length() <= 1) {
            return 0;
        }

        int len = s.length();
        // dp[i] represents the minimum cuts needed for substring(0,i)
        int[] dp = new int[len];

        // isPalindrome[i][j] represents whether substring from index i to j is palindrome
        boolean[][] isPalindrome = new boolean[len][len];

        // Initialize dp array with worst case scenario
        // For string length i, worst case needs i cuts (cutting after each character)
        for (int i = 0; i < len; i++) {
            dp[i] = i; // For example: "abcd" worst case is "a|b|c|d" (3 cuts)
        }

        // Main loop: j is the right boundary of the substring we're examining
        for (int j = 0; j < len; j++) {
            // i is the left boundary, checking all possible left boundaries up to j
            // For example, when j=2 (checking "abc"):
            // i=0: check "abc"
            // i=1: check "bc"
            // i=2: check "c"
            for (int i = 0; i <= j; i++) {
                // Check if substring from i to j is palindrome:
                // 1. First and last characters must be same
                // 2. Either the substring length <= 3 (like "a", "aa", "aba")
                // 3. Or the inner substring (i+1 to j-1) must be palindrome
                if (s.charAt(i) == s.charAt(j) && (j - i <= 2 || isPalindrome[i + 1][j - 1])) {
                    // Mark this substring as palindrome
                    isPalindrome[i][j] = true;

                    // Case 1: If substring starts from index 0
                    // No cuts needed as the whole substring is palindrome
                    if (i == 0) {
                        dp[j] = 0;
                    }
                    // Case 2: If substring starts after index 0
                    // Need to decide: either keep current minimum cuts
                    // or use (minimum cuts for substring(0,i-1) + 1 cut at position i)
                    else {
                        // dp[i-1] represents minimum cuts needed for substring(0,i-1)
                        // +1 represents one additional cut at position i
                        // Example: for "aab", when i=2,j=2:
                        // dp[1]("aa") = 0, so dp[2] = min(dp[2], 0+1) = 1
                        dp[j] = Math.min(dp[j], dp[i - 1] + 1);
                    }
                }
            }
        }

        // Return minimum cuts needed for the entire string
        return dp[len - 1];
    }

    public static void main(String[] args) {
        // Test Case 1: "aab" -> "aa|b" (1 cut)
        String s1 = "aab";
        System.out.println("Test Case 1 Result: " + minCut(s1)); // Expected: 1

        // Test Case 2: "a" -> "a" (0 cuts)
        String s2 = "a";
        System.out.println("Test Case 2 Result: " + minCut(s2)); // Expected: 0

        // Test Case 3: "ab" -> "a|b" (1 cut)
        String s3 = "ab";
        System.out.println("Test Case 3 Result: " + minCut(s3)); // Expected: 1

        // Test Case 4: "aaaa" -> "aaaa" (0 cuts as it's already palindrome)
        String s4 = "aaaa";
        System.out.println("Test Case 4 Result: " + minCut(s4)); // Expected: 0
    }
}

```





