---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1706. Where Will the Ball Fall"
date: "2025-02-15"
tags: Medium
categories:
  - "LeetCode Array"
---


- You have a 2-D `grid` of size `m x n` representing a box, and you have `n` balls. The box is open on the top and bottom sides.
- Each cell in the box has a diagonal board spanning two corners of the cell that can redirect a ball to the right or to the left.
  - A board that redirects the ball to the right spans the top-left corner to the bottom-right corner and is represented in the grid as `1`.
  - A board that redirects the ball to the left spans the top-right corner to the bottom-left corner and is represented in the grid as `-1`.
- We drop one ball at the top of each column of the box. Each ball can get stuck in the box or fall out of the bottom. A ball gets stuck if it hits a "V" shaped pattern between two boards or if a board redirects the ball into either wall of the box.
- Return *an array* `answer` *of size* `n` *where* `answer[i]` *is the column that the ball falls out of at the bottom after dropping the ball from the* `ith` *column at the top, or `-1` \*if the ball gets stuck in the box\*.*


**Example 1**

```
Input: grid = [ [1,1,1,-1,-1],[1,1,1,-1,-1],[-1,-1,-1,1,1],[1,1,1,1,-1],[-1,-1,-1,-1,-1] ]
Output: [1,-1,-1,-1,-1]
Explanation: This example is shown in the photo.
Ball b0 is dropped at column 0 and falls out of the box at column 1.
Ball b1 is dropped at column 1 and will get stuck in the box between column 2 and 3 and row 1.
Ball b2 is dropped at column 2 and will get stuck on the box between column 2 and 3 and row 0.
Ball b3 is dropped at column 3 and will get stuck on the box between column 2 and 3 and row 0.
Ball b4 is dropped at column 4 and will get stuck on the box between column 2 and 3 and row 1.
```

**Example 2**

```
Input: grid = [ [-1] ]
Output: [-1]
Explanation: The ball gets stuck against the left wall.
```

**Example 3**

```
Input: grid = [ [ 1,1,1,1,1,1],[-1,-1,-1,-1,-1,-1],[1,1,1,1,1,1],[-1,-1,-1,-1,-1,-1 ] ]
Output: [0,1,2,3,4,-1]
```

## Method 1

```tex
【O(n * m) time | O(n) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/02/15
 */
public class WhereWillTheBallFall {

    public static int[] findBall(int[][] grid) {
        // Get the width of the grid (number of columns)
        int n = grid[0].length;
        // Initialize result array to store the exit positions for each ball
        int[] ans = new int[n];

        // Simulate dropping a ball from each column
        for (int j = 0; j < n; j++) {
            // Track current column position of the ball
            // Initially, ball starts from column j
            int curCol = j;

            // Iterate through each row of the grid
            for (int[] row: grid) {
                // Get the direction of the current board
                // d = 1: board directs ball to right
                // d = -1: board directs ball to left
                int d = row[curCol];

                // Move ball to next position based on board direction
                curCol += d;

                // Check if ball gets stuck:
                // 1. curCol < 0: ball hits left boundary
                // 2. curCol == n: ball hits right boundary
                // 3. row[curCol] != d: forms V-shape with adjacent board
                //    Example: Current board goes right (1) but next position board goes left (-1)
                //    or current board goes left (-1) but next position board goes right (1)
                if (curCol < 0 || curCol == n || row[curCol] != d) {
                    // Ball is stuck, mark position as -1
                    curCol = -1;
                    // Stop simulating this ball's path
                    break;
                }
            }
            // Store final position for ball dropped from column j
            ans[j] = curCol;
        }
        return ans;
    }

    
    public static void main(String[] args) {
        // Test Case 1: Complex path with multiple directions
        int[][] grid1 = {
                {1, 1, 1, -1, -1},
                {1, 1, 1, -1, -1},
                {-1, -1, -1, 1, 1},
                {1, 1, 1, 1, -1},
                {-1, -1, -1, -1, -1}
        };
        System.out.println("Test Case 1: " + Arrays.toString(findBall(grid1)));

        // Test Case 2: Simplest case with single cell
        int[][] grid2 = { { -1 } };
        System.out.println("Test Case 2: " + Arrays.toString(findBall(grid2)));

        // Test Case 3: Alternating rows of same direction
        int[][] grid3 = {
                {1, 1, 1, 1, 1, 1},
                {-1, -1, -1, -1, -1, -1},
                {1, 1, 1, 1, 1, 1},
                {-1, -1, -1, -1, -1, -1}
        };
        System.out.println("Test Case 3: " + Arrays.toString(findBall(grid3)));
    }
}

```





