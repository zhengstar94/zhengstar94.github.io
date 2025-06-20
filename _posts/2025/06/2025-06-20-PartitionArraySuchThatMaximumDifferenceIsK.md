---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3443. Maximum Manhattan Distance After K Changes"
date: "2025-06-20"
tags: Medium
categories:
  - "LeetCode Greedy"
---


- You are given a string `s` consisting of the characters `'N'`, `'S'`, `'E'`, and `'W'`, where `s[i]` indicates movements in an infinite grid:
  - `'N'` : Move north by 1 unit.
  - `'S'` : Move south by 1 unit.
  - `'E'` : Move east by 1 unit.
  - `'W'` : Move west by 1 unit.
- Initially, you are at the origin `(0, 0)`. You can change **at most** `k` characters to any of the four directions.
- Find the **maximum** **Manhattan distance** from the origin that can be achieved **at any time** while performing the movements **in order**.
- The **Manhattan Distance** between two cells `(xi, yi)` and `(xj, yj)` is `|xi - xj| + |yi - yj|`.

**Example 1**

```
Input: s = "NWSE", k = 1

Output: 3

Explanation:

Change s[2] from 'S' to 'N'. The string s becomes "NWNE".
```

**Example 2**

```
Input: s = "NSWWEW", k = 3

Output: 6

Explanation:

Change s[1] from 'S' to 'N', and s[4] from 'E' to 'W'. The string s becomes "NNWWWW".

The maximum Manhattan distance from the origin that can be achieved is 6. Hence, 6 is the output.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Greedy;

/**
 * @Author zhengxingxing
 * @Date 2025/06/20
 */
public class MaximumManhattanDistanceAfterKChanges {
    
    public static int maxDistance(String s, int k) {
        // ans: stores the maximum Manhattan distance found so far
        int ans = 0;

        // x, y: current coordinates on the infinite grid (starting from origin 0,0)
        int x = 0, y = 0;

        // Simulate the movement step by step
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);

            // Update current position based on the movement character
            if (c == 'N') {
                y++;  // Move north: increase y-coordinate
            } else if (c == 'S') {
                y--;  // Move south: decrease y-coordinate  
            } else if (c == 'E') {
                x++;  // Move east: increase x-coordinate
            } else {  // c == 'W'
                x--;  // Move west: decrease x-coordinate
            }

            /*
             * CRITICAL FORMULA EXPLANATION:
             *
             * Math.abs(x) + Math.abs(y) + k * 2:
             * - This represents the THEORETICAL maximum distance we could achieve at this step
             * - Math.abs(x) + Math.abs(y): current actual Manhattan distance from origin
             * - k * 2: maximum additional distance we can gain through k character changes
             *   (each change can add up to 2 units of distance)
             *
             * i + 1:
             * - This represents the PHYSICAL maximum distance possible
             * - Since we've taken (i+1) steps total, we cannot be more than (i+1) units away from origin
             * - This is a fundamental physical constraint in any grid movement
             *
             * Math.min(theoretical_max, physical_max):
             * - We take the minimum because both constraints must be satisfied
             * - Even if we could theoretically reach distance 100 with our changes,
             *   if we've only taken 5 steps, we can't be more than 5 units away
             *
             * Example walkthrough:
             * s = "NWSE", k = 1, at step 3 (after "NWS"):
             * - Current position: (-1, 0), current distance = 1
             * - Theoretical max: 1 + 1*2 = 3 (current distance + max improvement from 1 change)
             * - Physical max: 3 (we've taken 3 steps)
             * - Actual max at this step: min(3, 3) = 3
             */
            ans = Math.max(ans, Math.min(Math.abs(x) + Math.abs(y) + k * 2, i + 1));
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test Case 1: Basic example showing the greedy approach
        String s1 = "NWSE";
        int k1 = 1;
        System.out.println("Test Case 1:");
        System.out.println("Input: s = \"" + s1 + "\", k = " + k1);
        System.out.println("Output: " + maxDistance(s1, k1));
        System.out.println("Expected: 3");
        System.out.println("Explanation: At step 2, current distance=2, 2+2*1=4, but only walked 2 steps, so min(4,2)=2");
        System.out.println("             At step 3, current distance=1, 1+2*1=3, walked 3 steps, so min(3,3)=3");
        System.out.println();

        // Test Case 2: More complex example with multiple changes
        String s2 = "NSWWEW";
        int k2 = 3;
        System.out.println("Test Case 2:");
        System.out.println("Input: s = \"" + s2 + "\", k = " + k2);
        System.out.println("Output: " + maxDistance(s2, k2));
        System.out.println("Expected: 6");
        System.out.println("Explanation: With 3 changes, we can potentially add up to 6 units of distance");
        System.out.println();
    }
}
```





