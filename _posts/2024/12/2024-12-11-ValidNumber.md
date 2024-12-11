---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "65. Valid Number"
date: "2024-12-11"
tags: Hard
categories:
  - "LeetCode Array"
---

- Given a string `s`, return whether `s` is a **valid number**.

- For example, all the following are valid numbers: `"2", "0089", "-0.1", "+3.14", "4.", "-.9", "2e10", "-90E3", "3e+7", "+6e-1", "53.5e93", "-123.456e789"`, while the following are not valid numbers: `"abc", "1a", "1e", "e3", "99e2.5", "--6", "-+3", "95a54e53"`.

- Formally, a **valid number** is defined using one of the following definitions:

  1. An **integer number** followed by an **optional exponent**.
  2. A **decimal number** followed by an **optional exponent**.

- An **integer number** is defined with an **optional sign** `'-'` or `'+'` followed by **digits**.

- A **decimal number** is defined with an **optional sign** `'-'` or `'+'` followed by one of the following definitions:

  1. **Digits** followed by a **dot** `'.'`.
  2. **Digits** followed by a **dot** `'.'` followed by **digits**.
  3. A **dot** `'.'` followed by **digits**.

- An **exponent** is defined with an **exponent notation** `'e'` or `'E'` followed by an **integer number**.

- The **digits** are defined as one or more digits.



**Example 1**

```
Input: s = "0"

Output: true
```

**Example 2**

```
Input: s = "e"

Output: false
```

**Example 3**

```
Input: s = "."

Output: false
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2024/12/11
 */
public class ValidNumber {
    public static boolean isNumber(String s) {
        // Check for null or empty input
        if (s == null || s.isEmpty()) {
            return false;
        }

        // Remove leading and trailing spaces
        s = s.trim();

        // Flags to track what parts of a valid number have been seen
        boolean seenDigit = false; // Tracks if a digit has been encountered
        boolean seenDot = false;  // Tracks if a dot '.' has been encountered
        boolean seenExp = false; // Tracks if an exponent 'e' or 'E' has been encountered
        boolean seenSign = false; // Tracks if a sign '+' or '-' has been encountered

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);

            if (Character.isDigit(c)) {
                // If the character is a digit, mark seenDigit as true
                seenDigit = true;
            } else if (c == '.') {
                // If the character is a dot:
                // 1. It is invalid if a dot or an exponent has already been encountered
                if (seenDot || seenExp) {
                    return false;
                }
                // 2. Mark seenDot as true
                seenDot = true;
            } else if (c == 'e' || c == 'E') {
                // If the character is an exponent:
                // 1. It is invalid if an exponent has already been encountered
                //    or if no digit has been encountered before the exponent
                if (seenExp || !seenDigit) {
                    return false;
                }
                // 2. Mark seenExp as true
                seenExp = true;
                // 3. Reset seenDigit and seenSign to ensure valid format after 'e'/'E'
                seenDigit = false; // A valid exponent must be followed by a number
                seenSign = false; // Allow a sign immediately after the exponent
            } else if (c == '+' || c == '-') {
                // If the character is a sign:
                // 1. It is invalid if a sign has already been encountered
                //    or if it is not at the start or immediately after an exponent
                if (seenSign || (i > 0 && s.charAt(i - 1) != 'e' && s.charAt(i - 1) != 'E')) {
                    return false;
                }
                // 2. Mark seenSign as true
                seenSign = true;
            } else {
                // If the character is anything else, it is invalid
                return false;
            }
        }

        // The string is valid only if a digit has been encountered
        return seenDigit;
    }

    public static void main(String[] args) {

        // Test cases to validate the implementation
        String[] testCases = {
                "0",       // Simple valid integer
                ".1",      // Valid floating point with leading dot
                "2e10",    // Valid scientific notation
                "-90E3",   // Valid negative number in scientific notation
                "3e+7",    // Valid positive exponent
                "+6e-1",   // Valid positive number with negative exponent
                "53.5e93", // Valid number with decimal and exponent
                "-123.456e789", // Valid negative number with decimal and exponent
                "abc",     // Invalid non-numeric string
                "1a",      // Invalid number with trailing characters
                "1e",      // Invalid exponent without digits after it
                "e3",      // Invalid string starting with exponent
                "99e2.5",  // Invalid exponent with decimal part
                "--6",     // Invalid double sign
                "-+3",     // Invalid mixed signs
                "95a54e53" // Invalid characters within the number
        };

        for (String testCase : testCases) {
            System.out.println("Input: " + testCase + " -> Output: " + isNumber(testCase));
        }
    }
}

```

