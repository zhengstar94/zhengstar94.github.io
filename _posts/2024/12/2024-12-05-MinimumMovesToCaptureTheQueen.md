---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3001. Minimum Moves to Capture The Queen"
date: "2024-12-05"
tags: Medium
categories:
  - "LeetCode Math"
---

- There is a **1-indexed** `8 x 8` chessboard containing `3` pieces.
- You are given `6` integers `a`, `b`, `c`, `d`, `e`, and `f` where:
  - `(a, b)` denotes the position of the white rook.
  - `(c, d)` denotes the position of the white bishop.
  - `(e, f)` denotes the position of the black queen.
- Given that you can only move the white pieces, return *the **minimum** number of moves required to capture the black queen*.
- **Note** that:
  - Rooks can move any number of squares either vertically or horizontally, but cannot jump over other pieces.
  - Bishops can move any number of squares diagonally, but cannot jump over other pieces.
  - A rook or a bishop can capture the queen if it is located in a square that they can move to.
  - The queen does not move.

**Example 1**

```
Input: a = 1, b = 1, c = 8, d = 8, e = 2, f = 3
Output: 2
Explanation: We can capture the black queen in two moves by moving the white rook to (1, 3) then to (2, 3).
It is impossible to capture the black queen in less than two moves since it is not being attacked by any of the pieces at the beginning.
```

**Example 2**

```
Input: a = 5, b = 3, c = 3, d = 4, e = 5, f = 2
Output: 1
Explanation: We can capture the black queen in a single move by doing one of the following: 
- Move the white rook to (5, 2).
- Move the white bishop to (5, 2).
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2024/12/05
 */
public class MinimumMovesToCaptureTheQueen {
    public static int minMovesToCaptureTheQueen(int a, int b, int c, int d, int e, int f) {
        if(
            // Rook is in the same column as the queen
                (a == e &&
                        // Two possible scenarios:
                        // 1. Rook and bishop are not in the same column, or
                        // 2. Bishop is positioned in a way that doesn't block the capture path
                        (a != c || (d - b) * (d - f) > 0)
                ) ||
                        // Alternative scenario: rook is in the same row as the queen
                        (b == f &&
                                // Two possible scenarios:
                                // 1. Rook and bishop are not in the same row, or
                                // 2. Bishop is positioned in a way that doesn't block the capture path
                                (b != d || (c - a) * (c - e) > 0)
                        )
        ) {
            // If any of the above conditions are true, the queen can be captured in one move
            return 1;
        }

        // Second condition: checking if the bishop can capture the queen in one move
        if(
            // Bishop and queen are on the same diagonal
                (Math.abs(c - e) == Math.abs(d - f)) &&
                        (
                                // Two additional conditions to ensure the rook doesn't block the capture:
                                // 1. Rook is not on the same diagonal line, or
                                // 2. Rook is completely outside the diagonal line between bishop and queen
                                ((c - e) * (b - f) != (a - e) * (d - f)) ||
                                        a < Math.min(c, e) ||
                                        a > Math.max(c, e)
                        )
        ) {
            // If the conditions are true, the queen can be captured in one move
            return 1;
        }

        return 2;
    }

    public static void main(String[] args) {
        // Test Case 1: Rook is in the same column as the queen
        // Rook at (1,1), Bishop at (2,2), Queen at (1,3)
        // Verifies if the rook can capture the queen in 1 move, avoiding the bishop
        System.out.println("Test 1: " + minMovesToCaptureTheQueen(1, 1, 2, 2, 1, 3) + " moves");

        // Test Case 2: Rook is in the same row as the queen
        // Rook at (1,1), Bishop at (2,2), Queen at (3,1)
        // Checks if the rook can capture the queen in 1 move, with no bishop interference
        System.out.println("Test 2: " + minMovesToCaptureTheQueen(1, 1, 2, 2, 3, 1) + " moves");

        // Test Case 3: Rook and bishop are on the diagonal with the queen
        // Rook at (2,2), Bishop at (1,1), Queen at (3,3)
        // Demonstrates a scenario requiring 2 moves to capture the queen
        System.out.println("Test 3: " + minMovesToCaptureTheQueen(2, 2, 1, 1, 3, 3) + " moves");

        // Test Case 4: Bishop is on the diagonal, rook can capture from outside
        // Rook at (1,1), Bishop at (3,3), Queen at (5,5)
        // Shows a scenario where the rook can capture in 1 move
        System.out.println("Test 4: " + minMovesToCaptureTheQueen(1, 1, 3, 3, 5, 5) + " moves");

        // Test Case 5: Another scenario with diagonal positioning
        // Rook at (4,2), Bishop at (6,4), Queen at (7,5)
        // Demonstrates another case of 1-move capture
        System.out.println("Test 5: " + minMovesToCaptureTheQueen(4, 2, 6, 4, 7, 5) + " moves");
    }
}

```





