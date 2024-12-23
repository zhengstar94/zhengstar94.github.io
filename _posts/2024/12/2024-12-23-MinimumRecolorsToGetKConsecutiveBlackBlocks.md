---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2379. Minimum Recolors to Get K Consecutive Black Blocks"
date: "2024-12-23"
tags: Easy
categories:
  - "LeetCode SlideWindow"
---


- You are given a **0-indexed** string `blocks` of length `n`, where `blocks[i]` is either `'W'` or `'B'`, representing the color of the `ith` block. The characters `'W'` and `'B'` denote the colors white and black, respectively.
- You are also given an integer `k`, which is the desired number of **consecutive** black blocks.
- In one operation, you can **recolor** a white block such that it becomes a black block.
- Return *the **minimum** number of operations needed such that there is at least **one** occurrence of* `k` *consecutive black blocks.*

**Example 1**

```
Input: blocks = "WBBWWBBWBW", k = 7
Output: 3
Explanation:
One way to achieve 7 consecutive black blocks is to recolor the 0th, 3rd, and 4th blocks
so that blocks = "BBBBBBBWBW". 
It can be shown that there is no way to achieve 7 consecutive black blocks in less than 3 operations.
Therefore, we return 3.
```

**Example 2**

```
Input: blocks = "WBWBBBW", k = 2
Output: 0
Explanation:
No changes need to be made, since 2 consecutive black blocks already exist.
Therefore, we return 0.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2024/12/23
 */
public class MinimumRecolorsToGetKConsecutiveBlackBlocks {
    public static int minimumRecolors(String blocks, int k) {
        int n = blocks.length();
        if (n == 0) {
            return 0;
        }

        // Initialize result with maximum possible value
        int minOperations = k;

        // Count of white blocks in current window (representing required operations)
        int whiteCount = 0;

        // Process the string using sliding window
        for (int i = 0; i < n; i++) {
            // Add current element to window
            if (blocks.charAt(i) == 'W') {
                whiteCount++;
            }

            // Skip until we have enough elements (k) in our window
            if (i < k - 1) {
                continue;
            }

            // Update minimum operations needed for current window
            minOperations = Math.min(minOperations, whiteCount);

            // Remove leftmost element for next iteration
            if (blocks.charAt(i - k + 1) == 'W') {
                whiteCount--;
            }
        }

        return minOperations;
    }

    public static void main(String[] args) {

        // Test Case 1: Example from the problem
        String test1 = "WBBWWBBWBW";
        int k1 = 7;
        System.out.println("Test Case 1:");
        System.out.println("Input: blocks = \"" + test1 + "\", k = " + k1);
        System.out.println("Expected Output: 3");
        System.out.println("Actual Output: " + minimumRecolors(test1, k1));
        System.out.println();

        // Test Case 2: No changes needed
        String test2 = "WBWBBBW";
        int k2 = 2;
        System.out.println("Test Case 2:");
        System.out.println("Input: blocks = \"" + test2 + "\", k = " + k2);
        System.out.println("Expected Output: 0");
        System.out.println("Actual Output: " + minimumRecolors(test2, k2));
        System.out.println();

        // Test Case 3: All white blocks
        String test3 = "WWWWW";
        int k3 = 3;
        System.out.println("Test Case 3:");
        System.out.println("Input: blocks = \"" + test3 + "\", k = " + k3);
        System.out.println("Expected Output: 3");
        System.out.println("Actual Output: " + minimumRecolors(test3, k3));
        System.out.println();

        // Test Case 4: K equals string length
        String test4 = "WBWBW";
        int k4 = 5;
        System.out.println("Test Case 4:");
        System.out.println("Input: blocks = \"" + test4 + "\", k = " + k4);
        System.out.println("Expected Output: 3");
        System.out.println("Actual Output: " + minimumRecolors(test4, k4));
        System.out.println();
    }
}

```





