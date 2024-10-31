---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "5.Longest Palindromic Substring"
date: "2024-10-31"
categories:
  - "LeetCode Dynamic Programming"
---


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

## Method 2

```tex
【O(n^2) time | O(1) space】
```

```java

package Leetcode.TwoPointer;

/**
 * @author zhengstars
 * @date 2024/10/31
 */
public class LongestPalindromicSubstring {
    public static String longestPalindrome(String s) {
        // If the input string is null or has a length less than 1, return an empty string
        if(s == null || s.length() < 1) {
            return "";
        }

        // Initialize starting index of the longest palindrome and its maximum length
        int start = 0;
        int maxLen = 1;

        // Loop through each character in the string to consider it as a center of potential palindrome
        for (int i = 0; i < s.length(); i++) {
            // Check for longest palindrome with odd length (center at i)
            int len1 = expandAroundCenter(s, i, i);
            // Check for longest palindrome with even length (center between i and i+1)
            int len2 = expandAroundCenter(s, i, i + 1);

            // Take the maximum length found from odd and even cases
            int len = Math.max(len1, len2);

            // If we found a longer palindrome, update maxLen and starting index
            if (len > maxLen) {
                maxLen = len;
                // Calculate the starting index of the current longest palindrome
                // "len - 1" represents the number of characters on either side of the center.
                // Dividing it by 2 gives the left-side distance from i to start of the palindrome.
                start = i - (len - 1) / 2;
            }
        }

        // Return the substring starting from 'start' and with length 'maxLen'
        return s.substring(start, start + maxLen);
    }

    private static int expandAroundCenter(String s, int left, int right) {
        // Expand as long as the characters on the left and right match and stay within bounds
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        // Length is right - left - 1 because left and right have moved one extra position
        return right - left - 1;
    }

    public static void main(String[] args) {
// Test case 1: A normal case with palindrome in the middle
        String s1 = "babad";
        System.out.println("Output: " + longestPalindrome(s1));
        // Expected output: "bab" or "aba"

        // Test case 2: The entire string is a palindrome
        String s2 = "racecar";
        System.out.println("Output: " + longestPalindrome(s2));
        // Expected output: "racecar"

        // Test case 3: No palindrome longer than one character
        String s3 = "abc";
        System.out.println("Output: " + longestPalindrome(s3));
        // Expected output: "a" or "b" or "c"

        // Test case 4: Even-length palindrome
        String s4 = "cbbd";
        System.out.println("Output: " + longestPalindrome(s4));
        // Expected output: "bb"

        // Test case 5: Single character
        String s5 = "a";
        System.out.println("Output: " + longestPalindrome(s5));
        // Expected output: "a"

        // Test case 6: Empty string
        String s6 = "";
        System.out.println("Output: " + longestPalindrome(s6));
        // Expected output: ""
    }
}

```