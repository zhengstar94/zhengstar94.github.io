---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2271. Maximum White Tiles Covered by a Carpet"
date: "2025-01-23"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindow"
---



- You are given a 2D integer array `tiles` where `tiles[i] = [li, ri]` represents that every tile `j` in the range `li <= j <= ri` is colored white.
- You are also given an integer `carpetLen`, the length of a single carpet that can be placed **anywhere**.
- Return *the **maximum** number of white tiles that can be covered by the carpet*.

**Example 1**

```
Input: tiles = [ [1,5],[10,11],[12,18],[20,25],[30,32 ] ], carpetLen = 10
Output: 9
Explanation: Place the carpet starting on tile 10. 
It covers 9 white tiles, so we return 9.
Note that there may be other places where the carpet covers 9 white tiles.
It can be shown that the carpet cannot cover more than 9 white tiles.
```

**Example 2**

```
Input: tiles = [ [ 10,11],[1,1 ] ], carpetLen = 2
Output: 2
Explanation: Place the carpet starting on tile 10. 
It covers 2 white tiles, so we return 2.
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/01/23
 */
public class MaximumWhiteTilesCoveredByACarpet {
    public static int maximumWhiteTiles(int[][] tiles, int carpetLen) {
        // Sort tiles array by left endpoint in ascending order
        Arrays.sort(tiles, (a, b) -> a[0] - b[0]);

        int ans = 0;    // Store the maximum number of white tiles that can be covered
        int cover = 0;  // Track the current number of tiles covered
        int left = 0;   // Left pointer for the sliding window

        // Iterate through each tile interval
        for (int[] tile : tiles) {
            int tl = tile[0]; // Left endpoint of current interval
            int tr = tile[1]; // Right endpoint of current interval

            // Add number of tiles in current interval to cover
            // Formula: right - left + 1 gives the count of tiles in current interval
            cover += tr - tl + 1;

            // While the carpet cannot fully cover from left interval to current interval
            // Need to remove tiles from left side that are out of carpet's range
            while (tiles[left][1] + carpetLen - 1 < tr) {
                // Remove tiles count of the leftmost interval that's out of range
                cover -= tiles[left][1] - tiles[left][0] + 1;
                left++; // Move left pointer to next interval
            }

            // Calculate uncovered tiles within the current carpet placement
            // tr - carpetLen + 1: leftmost position carpet needs to start to cover tr
            // tiles[left][0]: actual leftmost position of current tiles
            // Difference gives number of tiles that cannot be covered
            int uncover = Math.max(tr - carpetLen + 1 - tiles[left][0], 0);

            // Update answer: maximum between current answer and (covered tiles - uncovered tiles)
            ans = Math.max(ans, cover - uncover);
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test Case 1: Complex case with multiple intervals
        int[][] tiles1 = { { 1, 5}, {10, 11}, {12, 18}, {20, 25}, {30, 32 } };
        int carpetLen1 = 10;
        System.out.println("Test case 1: " + maximumWhiteTiles(tiles1, carpetLen1)); // Expected output: 9

        // Test Case 2: Consecutive intervals with small gaps
        int[][] tiles2 = { { 1, 3}, {4, 6}, {8, 10}, {11, 13 } };
        int carpetLen2 = 5;
        System.out.println("Test case 2: " + maximumWhiteTiles(tiles2, carpetLen2));

        // Test Case 3: Intervals with large gaps
        int[][] tiles3 = { { 5, 10}, {15, 20}, {25, 30 } };
        int carpetLen3 = 15;
        System.out.println("Test case 3: " + maximumWhiteTiles(tiles3, carpetLen3));
    }
}

```





