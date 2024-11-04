---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "227.Basic Calculator II"
date: "2024-11-04"
categories:
  - "LeetCode Stack"
---

- Given a string `s` which represents an expression, *evaluate this expression and return its value*.
- The integer division should truncate toward zero.
- You may assume that the given expression is always valid. All intermediate results will be in the range of `[-231, 231 - 1]`.
- **Note:** You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

**Example 1**

```
Input: s = "3+2*2"
Output: 7
```

**Example 2**

```
Input: s = " 3/2 "
Output: 1
```

**Example 3**

```
Input: s = " 3+5 / 2 "
Output: 5
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.Stack;

/**
 * @author zhengstars
 * @date 2024/11/04
 */
public class BasicCalculatorII {
    public static int calculate(String s) {
        // Handle edge cases: empty or null string
        if (s == null || s.length() == 0) {
            return 0;
        }

        // Stack to store numbers. For subtraction, we store negative numbers
        Stack<Integer> stack = new Stack<>();
        // Initial sign is '+', used to track the previous operator
        char sign = '+';
        // Accumulator for building multi-digit numbers
        int num = 0;

        // Iterate through each character in the expression
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);

            // If current character is a digit, build the number
            // For example: "14" -> first digit '1': num = 0*10 + 1 = 1
            //                  -> second digit '4': num = 1*10 + 4 = 14
            if (Character.isDigit(c)) {
                num = num * 10 + (c - '0');
            }

            // Process the number when we hit an operator or reach the end of string
            // The condition "i == s.length() - 1" ensures we process the last number
            if (!Character.isDigit(c) && c != ' ' || i == s.length() - 1) {
                // Process the number based on the previous operator (sign)
                switch (sign) {
                    case '+':
                        // For addition, simply push the number
                        stack.push(num);
                        break;
                    case '-':
                        // For subtraction, push the negative number
                        // This converts "a-b" to "a+(-b)"
                        stack.push(-num);
                        break;
                    case '*':
                        // For multiplication, multiply with previous number
                        // Pop the previous number, multiply, and push result
                        stack.push(stack.pop() * num);
                        break;
                    case '/':
                        // For division, divide the previous number
                        // Pop the previous number, divide, and push result
                        stack.push(stack.pop() / num);
                        break;
                }
                // Update sign for next iteration
                sign = c;
                // Reset number accumulator
                num = 0;
            }
        }

        // Calculate final result by summing all numbers in stack
        // For example: 14-3*2 -> stack will contain [14, -6]
        // Final result will be 14 + (-6) = 8
        int result = 0;
        while (!stack.isEmpty()) {
            result += stack.pop();
        }

        return result;
    }

    public static void main(String[] args) {
        // Test cases array
        // Each expression demonstrates different operator combinations
        String[] expressions = {
                "3+2*2",      // Tests multiplication precedence: 3+(2*2) = 7
                "14-3*2",     // Tests subtraction and multiplication: 14+(-3*2) = 8
                "100-20*3+2", // Tests complex expression: 100+(-20*3)+2 = 42
                "3/2",        // Tests division: = 1 (integer division truncates)
                "2*3*4"       // Tests multiple multiplications: = 24
        };

        // Process each test case and print results
        for (String exp : expressions) {
            int result = calculate(exp);
            System.out.println("表达式: " + exp + " = " + result);
        }
    }
}
```





