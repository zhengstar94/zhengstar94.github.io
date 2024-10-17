---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "386.Lexicographical Numbers"
date: "2024-10-18"
categories:
  - "LeetCode Backtracking"
---


- Given an integer `n`, return all the numbers in the range `[1, n]` sorted in lexicographical order.
- You must write an algorithm that runs in `O(n)` time and uses `O(1)` extra space.


**Example 1**

```
Input: n = 13
Output: [1,10,11,12,13,2,3,4,5,6,7,8,9]
```

**Example 2**

```
Input: n = 2
Output: [1,2]
```

## Method 1

```tex
【O(n) time | O(n)) space】
```

```java
package Leetcode.Backtracking;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2024/10/17
 */
public class LexicographicalNumbers {

    public static List<Integer> lexicalOrder(int n) {
        // Create a list to store the result
        List<Integer> result = new ArrayList<>();

        // Start DFS from each digit from 1 to 9
        for (int i = 1; i <= 9; i++) {
            dfs(i, n, result);
        }
        return result; // Return the result list
    }

    /**
     * Performs a depth-first search to generate numbers in lexicographical order.
     *
     * @param current the current number being formed
     * @param n the upper limit for valid numbers
     * @param result the list to store the valid lexicographical numbers
     */
    private static void dfs(int current, int n, List<Integer> result) {
        // If the current number exceeds n, terminate this branch of recursion
        if (current > n) {
            return;
        }

        // Add the current number to the result list
        result.add(current);

        // Attempt to extend the current number by adding digits 0 to 9
        for (int i = 0; i <= 9; i++) {
            // Generate the next number by appending the digit i
            int next = current * 10 + i;

            // If the next number exceeds n, stop further exploration
            if (next > n) {
                return;
            }

            // Recursively call dfs with the next number
            dfs(next, n, result);
        }
    }

    public static void main(String[] args) {
        // Test case 1: n = 13
        int n1 = 13;
        List<Integer> result1 = lexicalOrder(n1);
        System.out.println("Test case 1 (n = 13): " + result1);

    }
}

```
