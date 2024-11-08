---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1249.Minimum Remove to Make Valid Parentheses"
date: "2024-11-08"
categories:
  - "LeetCode Stack"
---

- Given a string s of `'('` , `')'` and lowercase English characters.
- Your task is to remove the minimum number of parentheses ( `'('` or `')'`, in any positions ) so that the resulting *parentheses string* is valid and return **any** valid string.
- Formally, a *parentheses string* is valid if and only if:
  - It is the empty string, contains only lowercase characters, or
  - It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
  - It can be written as `(A)`, where `A` is a valid string.

**Example 1**

```
Input: s = "lee(t(c)o)de)"
Output: "lee(t(c)o)de"
Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.
```

**Example 2**

```
Input: s = "a)b(c)d"
Output: "ab(c)d"
```

**Example 3**

```
Input: s = "))(("
Output: ""
Explanation: An empty string is also valid.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

/**
 * @author zhengxingxing
 * @date 2024/11/08
 */
public class MinimumRemoveToMakeValidParentheses {
    public static String minRemoveToMakeValid(String s) {
        // StringBuilder for first pass result
        StringBuilder sb = new StringBuilder();

        // Counter for total left parentheses seen
        int openSeen = 0;
        // Counter for currently unmatched left parentheses
        int balance = 0;

        // First pass: Remove invalid right parentheses
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);

            if (c == '(') {
                // Increment both counters when seeing a left parenthesis
                openSeen++;    // Track total left parentheses
                balance++;     // Track unmatched left parentheses
            } else if (c == ')') {
                // Skip this right parenthesis if there's no matching left parenthesis
                if (balance == 0) {
                    continue;
                }
                // Found a matching pair, decrease unmatched count
                balance--;
            }
            // Add current character to result
            sb.append(c);
        }

        // StringBuilder for final result
        StringBuilder result = new StringBuilder();
        // Calculate how many left parentheses we need to keep
        // openToKeep = total left parentheses - unmatched left parentheses
        int openToKeep = openSeen - balance;

        // Second pass: Remove excess left parentheses
        for (int i = 0; i < sb.length(); i++) {
            char c = sb.charAt(i);
            if (c == '(') {
                // For each left parenthesis, decrease the count to keep
                openToKeep--;
                // Skip this left parenthesis if we already have enough
                if (openToKeep < 0) {
                    continue;
                }
            }
            // Add current character to final result
            result.append(c);
        }

        return result.toString();
    }

    public static void main(String[] args) {
        // Test Case 1: Contains invalid right and left parentheses
        String test1 = "lee(t(c)o)de)";
        System.out.println("Test Case 1:");
        System.out.println("Input: " + test1);
        System.out.println("Output: " + minRemoveToMakeValid(test1));
        System.out.println();

        // Test Case 2: Contains invalid right parenthesis
        String test2 = "a)b(c)d";
        System.out.println("Test Case 2:");
        System.out.println("Input: " + test2);
        System.out.println("Output: " + minRemoveToMakeValid(test2));
        System.out.println();

        // Test Case 3: All parentheses are invalid
        String test3 = "))((";
        System.out.println("Test Case 3:");
        System.out.println("Input: " + test3);
        System.out.println("Output: " + minRemoveToMakeValid(test3));
        System.out.println();

        // Test Case 4: Only right parentheses
        String test4 = "a)))b";
        System.out.println("Test Case 4:");
        System.out.println("Input: " + test4);
        System.out.println("Output: " + minRemoveToMakeValid(test4));
        System.out.println();

        // Test Case 5: Complex nested parentheses
        String test5 = "(a(b(c)d)e)))";
        System.out.println("Test Case 5:");
        System.out.println("Input: " + test5);
        System.out.println("Output: " + minRemoveToMakeValid(test5));
        System.out.println();

        // Test Case 6: Empty string
        String test6 = "";
        System.out.println("Test Case 6:");
        System.out.println("Input: " + test6);
        System.out.println("Output: " + minRemoveToMakeValid(test6));
        System.out.println();

        // Test Case 7: Only letters
        String test7 = "abcde";
        System.out.println("Test Case 7:");
        System.out.println("Input: " + test7);
        System.out.println("Output: " + minRemoveToMakeValid(test7));
    }
}
```





