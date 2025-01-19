---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2266. Count Number of Texts"
date: "2025-01-19"
tags: Medium Review
categories:
  - "LeetCode Dynamic Programming"
---

- Alice is texting Bob using her phone. The **mapping** of digits to letters is shown in the figure below.
- In order to **add** a letter, Alice has to **press** the key of the corresponding digit `i` times, where `i` is the position of the letter in the key.
  - For example, to add the letter `'s'`, Alice has to press `'7'` four times. Similarly, to add the letter `'k'`, Alice has to press `'5'` twice.
  - Note that the digits `'0'` and `'1'` do not map to any letters, so Alice **does not** use them.
- However, due to an error in transmission, Bob did not receive Alice's text message but received a **string of pressed keys** instead.
  - For example, when Alice sent the message `"bob"`, Bob received the string `"2266622"`.
- Given a string `pressedKeys` representing the string received by Bob, return *the **total number of possible text messages** Alice could have sent*.
- Since the answer may be very large, return it **modulo** `10^9 + 7`.

**Example 1**

```
Input: pressedKeys = "22233"
Output: 8
Explanation:
The possible text messages Alice could have sent are:
"aaadd", "abdd", "badd", "cdd", "aaae", "abe", "bae", and "ce".
Since there are 8 possible messages, we return 8.
```

**Example 2**

```
Input: pressedKeys = "222222222222222222222222222222222222"
Output: 82876089
Explanation:
There are 2082876103 possible text messages Alice could have sent.
Since we need to return the answer modulo 10^9 + 7, we return 2082876103 % (10^9 + 7) = 82876089.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * @author zhengxingxing
 * @date 2025/01/19
 */
public class CountNumberOfTexts {

    // Define the modulus as required by the problem to avoid overflow
    private static final int MOD = 1_000_000_007;

    // Method to calculate the total number of text messages that can be formed
    public static int countTexts(String pressedKeys) {
        int n = pressedKeys.length(); // Get the length of the input string pressedKeys
        long[] dp = new long[n + 1];  // Create a dp array of size n+1 to store results for subproblems
        dp[0] = 1;  // Base case: An empty sequence has exactly one way to form it (do nothing)

        // Iterate from the 1st pressed key to the nth pressed key
        for (int i = 1; i <= n; i++) {
            // Add the number of ways using the last single key press (always valid)
            dp[i] = dp[i - 1]; // Single key usage, inherits from dp[i-1]

            // Check if the current key can combine with the previous key
            if (i >= 2 && pressedKeys.charAt(i - 1) == pressedKeys.charAt(i - 2)) {
                // If they are the same, add the values from dp[i-2] (combining two keys)
                dp[i] = (dp[i] + dp[i - 2]) % MOD;
            }

            // Check if the current key can combine with the last two keys (three keys total)
            if (i >= 3 && pressedKeys.charAt(i - 1) == pressedKeys.charAt(i - 2)
                    && pressedKeys.charAt(i - 2) == pressedKeys.charAt(i - 3)) {
                // If three consecutive keys are the same, add the values from dp[i-3]
                dp[i] = (dp[i] + dp[i - 3]) % MOD;
            }

            // Check if the current key can combine with the last three keys (four keys total)
            // This is only valid for the keys '7' and '9', as they can form up to 4-letter combinations
            if (i >= 4 && (pressedKeys.charAt(i - 1) == '7' || pressedKeys.charAt(i - 1) == '9')
                    && pressedKeys.charAt(i - 1) == pressedKeys.charAt(i - 2)
                    && pressedKeys.charAt(i - 2) == pressedKeys.charAt(i - 3)
                    && pressedKeys.charAt(i - 3) == pressedKeys.charAt(i - 4)) {
                // If four consecutive keys are the same and the key is 7 or 9, add the values from dp[i-4]
                dp[i] = (dp[i] + dp[i - 4]) % MOD;
            }
        }

        // The final result is stored in dp[n], the number of ways to form messages from the entire string
        return (int) dp[n];
    }

    // Main method to test the implementation with examples
    public static void main(String[] args) {
        // Test case 1
        String pressedKeys1 = "22233";
        System.out.println("Test Case 1 Result: " + countTexts(pressedKeys1)); // Expected output: 8

        // Test case 2
        String pressedKeys2 = "222222222222222222222222222222222222";
        System.out.println("Test Case 2 Result: " + countTexts(pressedKeys2)); // Expected output: 82876089

        // Additional test case
        String pressedKeys3 = "2266622";
        System.out.println("Test Case 3 Result: " + countTexts(pressedKeys3)); // Expected output depends on the pattern
    }
}

```





