---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "394. Decode String"
date: "2024-12-01"
tags: Medium
categories:
  - "LeetCode Stack"
---


- Given an encoded string, return its decoded string.
- The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.
- You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k`. For example, there will not be input like `3a` or `2[4]`.
- The test cases are generated so that the length of the output will never exceed `10^5`.

**Example 1**

```
Input: s = "3[a]2[bc]"
Output: "aaabcbc"
```

**Example 2**

```
Input: s = "3[a2[c]]"
Output: "accaccacc"
```

**Example 3**

```
Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"
```

**Example 4**

```
Input: s = "abc3[cd]xyz"
Output: "abccdcdcdxyz"
```

## Method 1

```tex
【O(n * k) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.Stack;

/**
 * @author zhengxingxing
 * @date 2024/12/01
 */
public class DecodeString {
    public static String decodeString(String s) {
        // Stack to track the repeat counts for each nested level
        Stack<Integer> countStack = new Stack<>();

        // Stack to track previous strings at each nested level
        Stack<String> stringStack = new Stack<>();

        // Current number being processed (supports multi-digit numbers)
        int num = 0;

        // Current string being built at the current processing level
        StringBuilder currentString = new StringBuilder();

        // Process each character in the input string
        for (char c: s.toCharArray()) {
            // 1. Number Processing: Build multi-digit numbers
            if(Character.isDigit(c)){
                // Convert character to digit and accumulate
                // Handles multi-digit numbers like 10, 23 etc.
                // Example: '1' -> num = 0*10 + 1 = 1
                //          '23' -> num = 1*10 + 3 = 13
                num = num * 10 + (c - '0');
            }
            // 2. Left Bracket: Start of a new encoding block
            else if(c =='['){
                // Push current number of repeats to count stack
                // This allows tracking nested repeat counts
                countStack.push(num);

                // Push current string to string stack
                // Preserves previous string context before entering new block
                stringStack.push(currentString.toString());

                // Reset for new block processing
                num = 0;
                currentString = new StringBuilder();
            }
            // 3. Right Bracket: End of an encoding block, time to decode
            else if(c == ']'){
                // Retrieve repeat count for current block
                int repeatTimes = countStack.pop();

                // Retrieve previous string context
                // This handles nested and sequential encodings
                StringBuilder previousString = new StringBuilder(stringStack.pop());

                // Create a new string to store repeated content
                StringBuilder repeatedString = new StringBuilder();

                // Repeat current block string specified number of times
                // Example: If currentString is "abc" and repeatTimes is 3
                // repeatedString becomes "abcabcabc"
                for (int i = 0; i < repeatTimes; i++) {
                    repeatedString.append(currentString);
                }

                // Combine previous context with repeated string
                // This resolves nested and sequential encodings
                currentString = previousString.append(repeatedString);
            }
            // 4. Letter Processing: Accumulate characters in current string
            else{
                currentString.append(c);
            }
        }

        // Return the fully decoded string
        return currentString.toString();
    }

    public static void main(String[] args) {
        // Comprehensive test cases covering various scenarios
        String[] testCases = {
                "3[a]2[bc]",           // Simple repetition
                "3[a2[c]]",             // Nested repetition
                "2[abc]3[cd]ef",        // Multi-level repetition
                "abc3[cd]xyz",          // Repetition with prefix/suffix
                "10[a]",                // Multi-digit number
                ""                      // Empty string
        };

        // Execute and print results for each test case
        for (String testCase : testCases) {
            System.out.println("Original Encoded String: " + testCase);
            try {
                String decoded = decodeString(testCase);
                System.out.println("Decoded Result: " + decoded);
            } catch (Exception e) {
                System.out.println("Decoding Error: " + e.getMessage());
            }
            System.out.println("-------------------");
        }
    }
}

```





