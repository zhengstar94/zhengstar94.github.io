---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2684. Maximum Number of Moves in a Grid"
date: "2025-06-29"
tags: Medium
categories:
  - "LeetCode Graphs"
---


- You are given a **0-indexed** `m x n` matrix `grid` consisting of **positive** integers.
- You can start at **any** cell in the first column of the matrix, and traverse the grid in the following way:
  - From a cell `(row, col)`, you can move to any of the cells: `(row - 1, col + 1)`, `(row, col + 1)` and `(row + 1, col + 1)` such that the value of the cell you move to, should be **strictly** bigger than the value of the current cell.
- Return *the **maximum** number of **moves** that you can perform.*

**Example 1**

```
Input: grid = [ [ 2,4,3,5],[5,4,9,3],[3,4,2,11],[10,9,13,15 ] ]
Output: 3
Explanation: We can start at the cell (0, 0) and make the following moves:
- (0, 0) -> (0, 1).
- (0, 1) -> (1, 2).
- (1, 2) -> (2, 3).
It can be shown that it is the maximum number of moves that can be made.
```

**Example 2**

```
Input: grid = [ [ 3,2,4],[2,1,9],[1,1,7 ] ]
Output: 0
Explanation: Starting from any cell in the first column we cannot perform any moves.
```

## Method 1

```tex
【O(m * n) time | O(m * n) space】
```

```java
package Leetcode.Graphs;

import java.util.Arrays;

/**
 * @Author zhengxingxing
 * @Date 2025/06/29
 */
public class MaximumNumberOfMovesInAGrid {
   
    public static int maxMoves(int[][] grid) {
        // Get the number of rows (m) and columns (n) in the grid
        int m = grid.length, n = grid[0].length;
        // Create a memoization array to store the maximum moves from each cell
        int[][] memo = new int[m][n];
        // Initialize the memo array with -1, meaning no cell has been computed yet
        for (int i = 0; i < m; i++) {
            Arrays.fill(memo[i], -1);
        }
        // Variable to keep track of the overall maximum moves
        int res = 0;
        // Try starting from every cell in the first column
        for (int row = 0; row < m; row++) {
            // Update the result with the maximum moves from this starting cell
            res = Math.max(res, dfs(grid, memo, row, 0));
        }
        // Return the maximum number of moves found
        return res;
    }
    
    private static int dfs(int[][] grid, int[][] memo, int row, int col) {
        // Get the grid dimensions
        int m = grid.length, n = grid[0].length;
        // If we are already at the last column, no more moves can be made
        if (col == n - 1) {
            return 0;
        }
        // If this cell's result has already been computed, return it directly
        if (memo[row][col] != -1) {
            return memo[row][col];
        }
        // Initialize the maximum moves from this cell as 0
        int max = 0;
        // Define the three possible row directions: up, straight, down
        int[] directions = {-1, 0, 1};
        // Try all three possible moves: right-up, right, right-down
        for (int dir : directions) {
            // Calculate the next row and next column after the move
            int nextRow = row + dir;
            int nextCol = col + 1;
            // Check if the next cell is within the grid and its value is strictly greater
            if (nextRow >= 0 && nextRow < m && grid[nextRow][nextCol] > grid[row][col]) {
                // Recursively compute the moves from the next cell and add 1 for this move
                int steps = 1 + dfs(grid, memo, nextRow, nextCol);
                // Take the maximum among all possible moves from this cell
                max = Math.max(max, steps);
            }
        }
        // Store the computed result in the memo array for future reference
        memo[row][col] = max;
        // Return the maximum moves from this cell
        return max;
    }
    
    public static void main(String[] args) {
        // Test case 1
        int[][] grid1 = {
                {2,4,3,5},
                {5,4,9,3},
                {3,4,2,11},
                {10,9,13,15}
        };
        // Test case 2
        int[][] grid2 = {
                {3,2,4},
                {2,1,9},
                {1,1,7}
        };
        // Output the results for both test cases
        System.out.println(maxMoves(grid1)); // Expected output: 3
        System.out.println(maxMoves(grid2)); // Expected output: 0
    }
}

```





