---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "224. Basic Calculator"
date: "2024-12-03"
tags: Hard
categories:
  - "LeetCode Stack"
---


- Given a string `s` representing a valid expression, implement a basic calculator to evaluate it, and return *the result of the evaluation*.
- **Note:** You are **not** allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

**Example 1**

```
Input: s = "1 + 1"
Output: 2
```

**Example 2**

```
Input: s = " 2-1 + 2 "
Output: 3
```

**Example 3**

```
Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.Stack;

/**
 * @author zhengxingxing
 * @date 2024/12/03
 */
public class BasicCalculator {
    public static int calculate(String s) {
        // Stack to store intermediate results and signs for nested expressions
        Stack<Integer> stack = new Stack<>();

        // Current number being parsed from the input string
        int currentNum = 0;

        // Running total of the calculation
        int result = 0;

        // Sign of the current number (1 for positive, -1 for negative)
        int sign = 1;

        // Iterate through each character in the input string
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);

            // Digit Handling: Build multi-digit numbers
            // If the character is a digit, update currentNum
            // Explanation:
            // - 10 * currentNum shifts existing digits left
            // - (ch - '0') converts char digit to its integer value
            // Example:
            //   If currentNum is 12 and ch is '3'
            //   12 * 10 = 120
            //   '3' - '0' = 3
            //   Result: 123
            if (Character.isDigit(ch)) {
                currentNum = 10 * currentNum + (ch - '0');
            }

            // Positive Sign '+' Handling
            // When '+' is encountered:
            // 1. Add current number to result with its sign
            // 2. Reset currentNum for next number
            // 3. Set sign to positive (1)
            else if (ch == '+') {
                result += sign * currentNum;
                currentNum = 0;
                sign = 1;
            }

            // Negative Sign '-' Handling
            // When '-' is encountered:
            // 1. Add current number to result with its sign
            // 2. Reset currentNum for next number
            // 3. Set sign to negative (-1)
            else if (ch == '-') {
                result += sign * currentNum;
                currentNum = 0;
                sign = -1;
            }

            // Opening Parenthesis '(' Handling
            // When '(' is encountered:
            // 1. Push current result to stack (to be restored later)
            // 2. Push current sign to stack
            // 3. Reset result and sign for nested expression
            else if (ch == '(') {
                stack.push(result);    // Store previous result
                stack.push(sign);      // Store previous sign

                result = 0;    // Reset for nested calculation
                sign = 1;      // Reset sign to positive
            }

            // Closing Parenthesis ')' Handling
            // When ')' is encountered:
            // 1. Add current number to result with its sign
            // 2. Reset currentNum
            // 3. Multiply result by previous sign from stack
            // 4. Add previous result from stack
            else if (ch == ')') {
                result += sign * currentNum;
                currentNum = 0;

                // Restore previous sign and multiply current result
                result *= stack.pop();

                // Add back the previous result from before the parenthesis
                result += stack.pop();
            }
        }

        // Handle the last number if it exists
        // This is necessary because the last number might not be followed by an operator
        if (currentNum != 0) {
            result += sign * currentNum;
        }

        return result;
    }

    public static void main(String[] args) {
        // Test Cases demonstrating various scenarios
        // Test Case 1: Simple addition and subtraction
        String test1 = "1+2-3";
        System.out.println("Test1: " + test1 + " = " + calculate(test1));

        // Test Case 2: Expression with simple parentheses
        String test2 = "1+(2-3)";
        System.out.println("Test2: " + test2 + " = " + calculate(test2));

        // Test Case 3: Expression with nested parentheses
        String test3 = "1+(2- (3 - 4) + 1)";
        System.out.println("Test3: " + test3 + " = " + calculate(test3));

        // Test Case 4: Complex parentheses nesting
        String test4 = "2-(5-6)";
        System.out.println("Test4: " + test4 + " = " + calculate(test4));

        // Test Case 5: Negative number scenario
        String test5 = "-(3-1)";
        System.out.println("Test5: " + test5 + " = " + calculate(test5));
    }
}

```





