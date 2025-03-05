---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2579. Count Total Number of Colored Cells"
date: "2025-03-05"
tags: Medium
categories:
  - "LeetCode Math" 
---


- There exists an infinitely large two-dimensional grid of uncolored unit cells. You are given a positive integer `n`, indicating that you must do the following routine for `n` minutes:
  - At the first minute, color **any** arbitrary unit cell blue.
  - Every minute thereafter, color blue **every** uncolored cell that touches a blue cell.
- Return *the number of **colored cells** at the end of* `n` *minutes*.

**Example 1**

```
Input: n = 1
Output: 1
Explanation: After 1 minute, there is only 1 blue cell, so we return 1.
```

**Example 2**

```
Input: n = 2
Output: 5
Explanation: After 2 minutes, there are 4 colored cells on the boundary and 1 in the center, so we return 5. 
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/03/05
 */
public class CountTotalNumberOfColoredCells {
    public static long coloredCells(int n) {
        // Mathematical formula breakdown:
        // 1. At minute 1: we have 1 cell
        // 2. For each subsequent minute i (i>1): we add 4*(i-1) new cells
        // 3. Total new cells = 4 * (1 + 2 + ... + (n-1))
        // 4. Using arithmetic sequence sum formula: 4 * (n-1)*n/2
        // 5. Simplifying: 2*n*(n-1)
        // 6. Final formula: 1 + 2*n*(n-1)
        // Note: Using 'L' suffix to prevent integer overflow for large n

        return n == 1 ? 1L : 1L + 2L * n * (n - 1);

        // Detailed calculation example for n=3:
        // Minute 1: 1 cell
        // Minute 2: adds 4*(2-1) = 4 cells
        // Minute 3: adds 4*(3-1) = 8 cells
        // Total = 1 + 4 + 8 = 13 cells
        // Using formula: 1 + 2*3*(3-1) = 1 + 2*3*2 = 1 + 12 = 13
    }

    public static void main(String[] args) {
        // Test case 1: Minimum input
        int n1 = 1;
        System.out.println("Test case 1 - Input: n = " + n1);
        System.out.println("Output: " + coloredCells(n1));

        // Test case 2: Basic example
        int n2 = 2;
        System.out.println("\nTest case 2 - Input: n = " + n2);
        System.out.println("Output: " + coloredCells(n2));

        // Test case 3: Verify pattern
        int n3 = 3;
        System.out.println("\nTest case 3 - Input: n = " + n3);
        System.out.println("Output: " + coloredCells(n3));

        // Test case 4: Large number to verify no integer overflow
        int n4 = 1000;
        System.out.println("\nTest case 4 - Input: n = " + n4);
        System.out.println("Output: " + coloredCells(n4));
    }
}

```





