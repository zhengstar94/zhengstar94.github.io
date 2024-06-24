---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "20. Valid Parentheses"
date: "2024-02-24"
categories:
  - "LeetCode Stack"
---

# LeetCode 20. Valid Parentheses 

- Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.
- An input string is valid if:
  1. Open brackets must be closed by the same type of brackets.
  2. Open brackets must be closed in the correct order.
  3. Every close bracket has a corresponding open bracket of the same type.

**Example 1**

```
Input: s = "()"
Output: true
```

**Example 2**

```
Input: s = "()[]{}"
Output: true
```

**Example 3**

```
Input: s = "(]"
Output: false
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Stack;

/**
 * @author zhengstars
 * @date 2024/03/20
 */
public class ValidParentheses {
    public static boolean isValid(String s) {
        // Directly return false if the length of the string is odd
        // as it clearly cannot be valid with mismatched pairs of parentheses
        if (s.length() % 2 != 0) {
            return false;
        }

        char[] stack = new char[s.length()];
        int top = -1;

        // Traverse each character in the string
        for (char c : s.toCharArray()) {
            // Push the open parentheses onto the stack
            if (c == '(' || c == '[' || c == '{') {
                stack[++top] = c;
            } else {
                // Check if the open parentheses at the stack top
                // matches with the closed parentheses 
                if (top == -1 ||
                        (c == ')' && stack[top] != '(') ||
                        (c == ']' && stack[top] != '[') ||
                        (c == '}' && stack[top] != '{')) {
                    return false;
                }
                // Pop the matched open parentheses from the stack
                top--;
            }
        }
        // Return true if all open parentheses have been successfully matched
        // (i.e. the stack is empty), return false otherwise
        return top == -1;
    }

    public static void main(String[] args) {
        // Example 1
        String str1 = "()";
        System.out.println(isValid(str1));  // Output: true

        // Example 2
        String str2 = "()[]{}";
        System.out.println(isValid(str2));  // Output: true

        // Example 3
        String str3 = "(]";
        System.out.println(isValid(str3));  // Output: false

        // Example 4
        String str4 = "([)]";
        System.out.println(isValid(str4));  // Output: false
    }
}
```

