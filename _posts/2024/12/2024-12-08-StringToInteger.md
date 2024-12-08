---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "8. String to Integer (atoi)"
date: "2024-12-08"
tags: Medium
categories:
  - "LeetCode String"
---

- Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer.
- The algorithm for `myAtoi(string s)` is as follows:
  1. **Whitespace**: Ignore any leading whitespace (`" "`).
  2. **Signedness**: Determine the sign by checking if the next character is `'-'` or `'+'`, assuming positivity if neither present.
  3. **Conversion**: Read the integer by skipping leading zeros until a non-digit character is encountered or the end of the string is reached. If no digits were read, then the result is 0.
  4. **Rounding**: If the integer is out of the 32-bit signed integer range `[-231, 231 - 1]`, then round the integer to remain in the range. Specifically, integers less than `-231` should be rounded to `-231`, and integers greater than `231 - 1` should be rounded to `231 - 1`.
- Return the integer as the final result.

**Example 1**

```
Input: s = "42"

Output: 42

Explanation:

The underlined characters are what is read in and the caret is the current reader position.
Step 1: "42" (no characters read because there is no leading whitespace)
         ^
Step 2: "42" (no characters read because there is neither a '-' nor '+')
         ^
Step 3: "42" ("42" is read in)
				 ^
```

**Example 2**

```
Input: s = " -042"

Output: -42

Explanation:

Step 1: "   -042" (leading whitespace is read and ignored)
            ^
Step 2: "   -042" ('-' is read, so the result should be negative)
             ^
Step 3: "   -042" ("042" is read in, leading zeros ignored in the result)
						   ^
```

**Example 3**

```
Input: s = "1337c0d3"

Output: 1337

Explanation:

Step 1: "1337c0d3" (no characters read because there is no leading whitespace)
         ^
Step 2: "1337c0d3" (no characters read because there is neither a '-' nor '+')
         ^
Step 3: "1337c0d3" ("1337" is read in; reading stops because the next character is a non-digit)       ^
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2024/12/08
 */
public class StringToInteger {
    public static int myAtoi(String s) {
        // Step 1: Remove leading and trailing whitespaces
        // Ensures no unnecessary spaces affect conversion
        s = s.trim();

        // Step 2: Handle empty string case
        // If string is empty after trimming, return 0
        if(s.isEmpty()){
            return 0;
        }

        // Step 3: Sign handling
        // Default to positive, track sign and starting index
        int sign = 1;  // 1 for positive, -1 for negative
        int index = 0;

        // Check for explicit sign character
        if(s.charAt(index) == '-'){
            // Negative sign found
            sign = -1;
            index++;
        }else if(s.charAt(index) == '+'){
            // Positive sign found (optional)
            index++;
        }

        // Step 4: Digit conversion
        // Use long to prevent intermediate overflow during conversion
        long result = 0;

        // Continue reading digits until non-digit or end of string
        while (index < s.length() && Character.isDigit(s.charAt(index))){
            // Convert character digit to numeric value
            // Subtracting '0' converts character to its numeric equivalent
            int digit = s.charAt(index) - '0';

            // Step 5: Overflow Prevention
            // Two-condition check to detect potential 32-bit integer overflow
            // Condition 1: result already exceeds MAX_VALUE / 10
            // Condition 2: result equals MAX_VALUE / 10 and next digit exceeds MAX_VALUE's last digit
            if(result > Integer.MAX_VALUE / 10 ||
                    (result == Integer.MAX_VALUE / 10 && digit > Integer.MAX_VALUE % 10)
            ){
                // Handle overflow by returning boundary values
                // Depends on the sign of the number
                return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            }

            // Build number digit by digit
            // Multiply existing result by 10 and add new digit
            result = result * 10 + digit;
            index++;
        }

        // Final step: Apply sign and convert to 32-bit integer
        // Multiplies result by sign and casts to int
        return (int) (sign * result);
    }

    public static void main(String[] args) {

        // Test cases covering different scenarios
        String[] testCases = {
                "42",           // Simple positive number
                " -042",        // Negative number with leading zeros and spaces
                "1337c0d3",     // Number with non-digit characters
                "  +  413",     // Invalid number with spaces
                "words and 987",// Words before number
                "-91283472332", // Number beyond Integer.MIN_VALUE
                "91283472332"   // Number beyond Integer.MAX_VALUE
        };

        for (String testCase : testCases) {
            System.out.println("Input: \"" + testCase + "\"");
            System.out.println("Output: " + myAtoi(testCase));
            System.out.println();
        }
    }

}

```
