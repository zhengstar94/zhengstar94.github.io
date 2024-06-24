---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Reverse Polish Notation"
date: "2023-05-06"
categories:
  - "Algorithm Stack"
---

# Reverse Polish Notation [Medium]

- Evaluate the value of an arithmetic expression in Reverse Polish Notation.
- Valid operators are `+`, `-`, `*`, `/`. Each operand may be an integer or another expression.
- Note:
  - Division between two integers should truncate toward zero.
  - The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

**Sample Input 1**

```
["2", "1", "+", "3", "*"]
```

**Sample Output 1**

```
9 //((2 + 1) * 3) = 9
```

**Sample Input 2**

```
["4", "13", "5", "/", "+"]
```

**Sample Output 2**

```
6 // (4 + (13 / 5)) = 6
```

**Sample Input 3**

```
["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
```

**Sample Output 3**

```
22 //(10 * (6 / ((9 + 3) * -11))) + 17) + 5
// = ((10 * (6 / (12 * -11))) + 17) + 5
// = ((10 * (6 / -132)) + 17) + 5
// = ((10 * 0) + 17) + 5
// = (0 + 17) + 5
// = 17 + 5
// = 22
```

<br>

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Stack;

import java.util.Stack;

/**
 * @author zhengstars
 * @date 2023/06/04
 */
public class ReversePolishNotation {
    public static int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();

        for (String token : tokens) {
            // If the token is an operator
            if (isOperator(token)) {
                // Pop the top operand from the stack
                int operand2 = stack.pop();
                // Pop the second top operand from the stack
                int operand1 = stack.pop();
                // Perform the operation
                int result = performOperation(operand1, operand2, token);
                // Push the result back to the stack
                stack.push(result);
            } else { // If the token is an operand
                // Convert the token to an integer
                int operand = Integer.parseInt(token);
                // Push the operand to the stack
                stack.push(operand);
            }
        }

        // The final result is the only element remaining in the stack
        return stack.pop();
    }

    /**
     * Check if the token is an operator
     * @param token
     * @return
     */
    private static boolean isOperator(String token) {
        return token.equals("+") || token.equals("-") || token.equals("*") || token.equals("/");
    }

    /**
     * Perform the specified operation on the operands
     * @param operand1
     * @param operand2
     * @param operator
     * @return
     */
    private static int performOperation(int operand1, int operand2, String operator) {
        switch (operator) {
            case "+":
                // Addition
                return operand1 + operand2;
            case "-":
                // Subtraction
                return operand1 - operand2;
            case "*":
                // Multiplication
                return operand1 * operand2;
            case "/":
                // Integer division
                return operand1 / operand2;
            default:
                return 0;
        }
    }

    public static void main(String[] args) {
        String[] token = new String[]{"10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"};
        System.out.println(evalRPN(token));
    }

}
```

