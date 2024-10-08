---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "22.Generate Parenthesesn"
date: "2024-10-08"
categories:
  - "LeetCode Stack"
---

- Given `n` pairs of parentheses, write a function to *generate all combinations of well-formed parentheses*.


**Example 1**

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2**

```
Input: n = 1
Output: ["()"]
```

## Method 1

```tex
【O(4^n / √n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2024/10/08
 */
public class GenerateParentheses {

    public static List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        backtrac(result, new StringBuilder(), 0, 0, n);
        return result;
    }

    /**
     * Recursive backtracking method to generate parentheses combinations
     * @param result The list to store all valid combinations
     * @param current The current combination being built
     * @param open The count of open parentheses in the current combination
     * @param close The count of close parentheses in the current combination
     * @param max The maximum number of pairs allowed (n)
     */
    private static void backtrac(List<String> result, StringBuilder current, int open, int close, int max) {
        // Base case: If the current combination has the correct length, add it to the result
        if(current.length() == max * 2){
            result.add(current.toString());
            return;
        }

        // Add an open parenthesis if we haven't used all n open parentheses yet
        if(open < max){
            current.append("(");
            backtrac(result, current, open + 1, close, max);
            current.setLength(current.length() - 1);  // Backtrack: remove the last added parenthesis
        }

        // Add a close parenthesis if it's valid (close count < open count)
        if(close < open){
            current.append(")");
            backtrac(result, current, open, close + 1, max);
            current.setLength(current.length() - 1);  // Backtrack: remove the last added parenthesis
        }
    }

    /**
     * Main method to run tests
     */
    public static void main(String[] args) {
        // Test different values of n
        test(0);
        test(1);
        test(2);
        test(3);
        test(4);
    }

    /**
     * Test method to generate and print results for a given n
     * @param n The number of pairs of parentheses to generate
     */
    private static void test(int n) {
        List<String> result = generateParenthesis(n);
        System.out.println("n = " + n);
        System.out.println("Result: " + result);
        System.out.println("Number of combinations: " + result.size());
        System.out.println();
    }
}
```





