---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "647.Palindromic Substrings"
date: "2024-09-18"
categories:
  - "LeetCode Dynamic Programming"
---

# 647. Palindromic Substrings

- Given a string `s`, return *the number of **palindromic substrings** in it*.
- A string is a **palindrome** when it reads the same backward as forward.
- A **substring** is a contiguous sequence of characters within the string.

**Example 1**

```
Input: s = "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```

**Example 2**

```
Input: s = "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
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

    public static int countSubstrings(String s) {
        // Get the length of the input string
        int n = s.length();

        // Create a 2D boolean array to store whether substrings are palindromes
        // dp[i][j] will be true if the substring from index i to j is a palindrome
        boolean[][] dp = new boolean[n][n];

        // Initialize the count of palindromic substrings
        int count = 0;

        // Base case: All substrings of length 1 are palindromes
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
            count++;
        }

        // Check for substrings of length 2 and above
        for (int len = 2; len <= n; len++) {
            for (int i = 0; i < n - len + 1; i++) {
                // Calculate the ending index j based on the current starting index i and length
                int j = i + len - 1;

                // Check if the substring is a palindrome:
                // 1. The characters at both ends should be the same
                // 2. For length > 2, the inner substring (excluding the ends) should be a palindrome
                if (s.charAt(i) == s.charAt(j) && (len == 2 || dp[i+1][j-1])) {
                    dp[i][j] = true;
                    count++;
                }
            }
        }

        // Return the total count of palindromic substrings
        return count;
    }

}

```

## Method 2

```tex
【O(n^2) time | O(1) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * @author zhengstars
 * @date 2024/08/31
 */
public class LongestPalindromicSubstring {

    // Main method to count the number of palindromic substrings in the input string `s`
    public int countSubstrings(String s) {
        int count = 0;
        
        // Loop through each character of the string
        // Each character is considered the center of a potential palindrome
        for (int i = 0; i < s.length(); i++) {
            
            // Count odd-length palindromes with center at `i`
            count += expandAroundCenter(s, i, i); // Expand around a single center (odd-length palindrome)
            
            // Count even-length palindromes with center between `i` and `i + 1`
            count += expandAroundCenter(s, i, i + 1); // Expand around two adjacent characters (even-length palindrome)
        }
        
        return count; // Return the total count of palindromic substrings
    }
    
    // Helper method to expand around the center and count palindromes
    // `left` and `right` define the boundaries to expand from the center
    private int expandAroundCenter(String s, int left, int right) {
        int count = 0;
        
        // Expand while the characters at `left` and `right` are the same (palindrome condition)
        // and ensure we stay within the bounds of the string
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            count++;    // Increment count for each valid palindrome found
            left--;     // Move left pointer outward
            right++;    // Move right pointer outward
        }
        
        return count; // Return the count of palindromes found in this expansion
    }

}

```

