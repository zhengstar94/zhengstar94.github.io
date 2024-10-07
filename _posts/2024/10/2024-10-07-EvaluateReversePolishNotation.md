---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "150.Evaluate Reverse Polish Notation"
date: "2024-10-07"
categories:
  - "LeetCode Stack"
---

- You are given an array of strings `tokens` that represents an arithmetic expression in a [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).
- Evaluate the expression. Return *an integer that represents the value of the expression*.
- **Note** that:
  - The valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
  - Each operand may be an integer or another expression.
  - The division between two integers always **truncates toward zero**.
  - There will not be any division by zero.
  - The input represents a valid arithmetic expression in a reverse polish notation.
  - The answer and all the intermediate calculations can be represented in a **32-bit** integer.


**Example 1**

```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

**Example 2**

```
Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

**Example 3**

```
Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
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
 * @date 2024/10/07
 */
public class EvaluateReversePolishNotation {
    public static int evalRPN(String[] tokens) {
        // Initialize a stack to store operands
        Stack<Integer> stack = new Stack<>();

        // Iterate through each token in the input array
        for (String token : tokens) {
            if (token.equals("+")) {
                // Addition: pop two numbers, add them, and push the result
                stack.push(stack.pop() + stack.pop());
            } else if (token.equals("*")) {
                // Multiplication: pop two numbers, multiply them, and push the result
                stack.push(stack.pop() * stack.pop());
            } else if (token.equals("-")) {
                // Subtraction: pop two numbers (order matters), subtract, and push the result
                int b = stack.pop();
                int a = stack.pop();
                stack.push(a - b);
            } else if (token.equals("/")) {
                // Division: pop two numbers (order matters), divide, and push the result
                int b = stack.pop();
                int a = stack.pop();
                stack.push(a / b);
            } else {
                // If the token is a number, parse it to integer and push onto the stack
                stack.push(Integer.parseInt(token));
            }
        }

        // The final result is the only item left on the stack
        return stack.pop();
    }

    public static void main(String[] args) {
        // Test case 1: Basic addition and multiplication
        String[] tokens1 = { "2","1","+","3","*" };
        System.out.println("Test case 1 result: " + evalRPN(tokens1)); // Expected output: 9

        // Test case 2: Division and addition
        String[] tokens2 = { "4","13","5","/","+" };
        System.out.println("Test case 2 result: " + evalRPN(tokens2)); // Expected output: 6

        // Test case 3: Complex expression with multiple operations
        String[] tokens3 = { "10","6","9","3","+","-11","*","/","*","17","+","5","+" };
        System.out.println("Test case 3 result: " + evalRPN(tokens3)); // Expected output: 22

        // Test case 4: Expression with negative numbers
        String[] tokens4 = { "4","-2","/","2","-" };
        System.out.println("Test case 4 result: " + evalRPN(tokens4)); // Expected output: -4

        // Test case 5: Expression with a single number
        String[] tokens5 = { "18" };
        System.out.println("Test case 5 result: " + evalRPN(tokens5)); // Expected output: 18
    }
}
```





