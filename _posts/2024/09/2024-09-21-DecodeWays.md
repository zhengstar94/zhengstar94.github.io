---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "91.Decode Ways"
date: "2024-09-21"
categories:
  - "LeetCode Dynamic Programming"
---

# 91. Decode Ways

- You have intercepted a secret message encoded as a string of numbers. The message is **decoded** via the following mapping:

  ```"1" -> 'A' "2" -> 'B' ... "25" -> 'Y' "26" -> 'Z'```

- However, while decoding the message, you realize that there are many different ways you can decode the message because some codes are contained in other codes (`"2"` and `"5"` vs `"25"`).

  For example, `"11106"` can be decoded into:

  - `"AAJF"` with the grouping `(1, 1, 10, 6)`
  - `"KJF"` with the grouping `(11, 10, 6)`
  - The grouping `(1, 11, 06)` is invalid because `"06"` is not a valid code (only `"6"` is valid).

  Note: there may be strings that are impossible to decode.

  Given a string s containing only digits, return the **number of ways** to **decode** it. If the entire string cannot be decoded in any valid way, return `0`.

  The test cases are generated so that the answer fits in a **32-bit** integer.

**Example 1**

```
Input: s = "12"

Output: 2

Explanation:

"12" could be decoded as "AB" (1 2) or "L" (12).
```

**Example 2**

```
Input: s = "226"

Output: 3

Explanation:

"226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

**Example 3**

```
Input: s = "06"

Output: 0

Explanation:

"06" cannot be mapped to "F" because of the leading zero ("6" is different from "06"). In this case, the string is not a valid encoding, so return 0.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * @author zhengstars
 * @date 2024/09/21
 */
public class DecodeWays {
    /**
     * Calculates the number of ways to decode a string of digits.
     * @param s The input string containing digits.
     * @return The number of possible decodings.
     */
    public static int numDecodings(String s) {
        int n = s.length();
        // dp[i] represents the number of ways to decode the first i characters
        int[] dp = new int[n + 1];

        // Base case: empty string has one way to decode (do nothing)
        dp[0] = 1;

        // Iterate through each character in the string
        for (int i = 1; i <= n; i++) {
            // Check if single digit decoding is possible
            if (s.charAt(i - 1) != '0') {
                // If the current digit is not '0', it can be decoded alone
                // So we add the number of ways to decode the string up to the previous position
                dp[i] += dp[i - 1];
            }

            // Check if two-digit decoding is possible
            if (i > 1 && s.charAt(i - 2) != '0' && twoDigitNum(s, i) <= 26) {
                // If the last two digits form a valid number (10-26),
                // we add the number of ways to decode the string up to two positions back
                dp[i] += dp[i - 2];
            }
        }

        // Return the total number of ways to decode the entire string
        return dp[n];
    }

    /**
     * Converts two consecutive digits in the string to an integer.
     * @param s The input string.
     * @param i The current position (we look at i-2 and i-1).
     * @return The integer value of the two digits.
     */
    private static int twoDigitNum(String s, int i) {
        // Convert char to int by subtracting '0'
        // Multiply the tens digit by 10 and add the ones digit
        return (s.charAt(i - 2) - '0') * 10 + (s.charAt(i - 1) - '0');
    }

    /**
     * Main method to test the solution with various test cases.
     */
    public static void main(String[] args) {

        // Test cases
        String[] testCases = {
                "12",    // Expected output: 2 (AB (1 2) or L (12))
                "226",   // Expected output: 3 (BZ (2 26) or VF (22 6) or BBF (2 2 6))
                "0",     // Expected output: 0 (cannot be decoded)
                "06",    // Expected output: 0 (cannot be decoded)
                "10",    // Expected output: 1 (J (10))
                "27",    // Expected output: 1 (BG (2 7))
                "234",   // Expected output: 3 (BCD (2 3 4) or WD (23 4) or BW (2 34))
                "1111"   // Expected output: 5 (AAAA, KAA, AKA, AAK, KK)
        };

        for (String testCase : testCases) {
            int result = numDecodings(testCase);
            System.out.println("Input: \"" + testCase + "\", Output: " + result);
        }
    }
}

```





