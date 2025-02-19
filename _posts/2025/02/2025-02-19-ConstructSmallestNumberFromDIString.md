---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2375. Construct Smallest Number From DI String"
date: "2025-02-19"
tags: Medium
categories:
  - "LeetCode Stack"
---

- You are given a **0-indexed** string `pattern` of length `n` consisting of the characters `'I'` meaning **increasing** and `'D'` meaning **decreasing**.
- A **0-indexed** string `num` of length `n + 1` is created using the following conditions:
  - `num` consists of the digits `'1'` to `'9'`, where each digit is used **at most** once.
  - If `pattern[i] == 'I'`, then `num[i] < num[i + 1]`.
  - If `pattern[i] == 'D'`, then `num[i] > num[i + 1]`.
- Return *the lexicographically **smallest** possible string* `num` *that meets the conditions.*

**Example 1**

```
Input: pattern = "IIIDIDDD"
Output: "123549876"
Explanation:
At indices 0, 1, 2, and 4 we must have that num[i] < num[i+1].
At indices 3, 5, 6, and 7 we must have that num[i] > num[i+1].
Some possible values of num are "245639871", "135749862", and "123849765".
It can be proven that "123549876" is the smallest possible num that meets the conditions.
Note that "123414321" is not possible because the digit '1' is used more than once.
```

**Example 2**

```
Input: pattern = "DDD"
Output: "4321"
Explanation:
Some possible values of num are "9876", "7321", and "8742".
It can be proven that "4321" is the smallest possible num that meets the conditions.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.ArrayDeque;
import java.util.Deque;

/**
 * Author: zhengxingxing
 * Date: 2025/02/19
 */
public class ConstructSmallestNumberFromDIString {
    // Method to construct the smallest number based on the given pattern
    public static String smallestNumber(String pattern) {
        StringBuilder result = new StringBuilder(); // StringBuilder to store the final result
        Deque<Integer> stack = new ArrayDeque<>();  // Stack to temporarily store digits

        // Iterate through the pattern (including one extra pass for the last number)
        for (int i = 0; i <= pattern.length(); i++) {
            stack.push(i + 1); // Push the current number (i+1) onto the stack.

            // If we encounter 'I' (increasing) in the pattern, OR we reach the end of the pattern:
            // Pop all numbers from the stack and append them to the result.
            if (i == pattern.length() || pattern.charAt(i) == 'I') {
                while (!stack.isEmpty()) {
                    result.append(stack.pop());
                }
            }
        }
        // Return the constructed smallest number as a string
        return result.toString();
    }

    // Main method - serves as an entry point to test the program
    public static void main(String[] args) {
        // Example test cases
        String pattern1 = "IIIDIDDD"; // Pattern: Increasing-Increasing-Increasing-Decreasing-Increasing-Decreasing-Decreasing-Decreasing
        String pattern2 = "DDD";      // Pattern: Decreasing-Decreasing-Decreasing

        // Outputs the smallest numbers corresponding to each pattern
        System.out.println("Input: " + pattern1 + " Output: " + smallestNumber(pattern1)); // Output: "123549876"
        System.out.println("Input: " + pattern2 + " Output: " + smallestNumber(pattern2)); // Output: "4321"
    }
}

```





