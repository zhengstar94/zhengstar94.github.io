---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "130. Surrounded Regions"
date: "2025-06-29"
tags: Medium
categories:
  - "LeetCode Graphs"
---


- You are given an `m x n` matrix `board` containing **letters** `'X'` and `'O'`, **capture regions** that are **surrounded**:
  - **Connect**: A cell is connected to adjacent cells horizontally or vertically.
  - **Region**: To form a region **connect every** `'O'` cell.
  - **Surround**: The region is surrounded with `'X'` cells if you can **connect the region** with `'X'` cells and none of the region cells are on the edge of the `board`.
- To capture a **surrounded region**, replace all `'O'`s with `'X'`s **in-place** within the original board. You do not need to return anything.

**Example 1**

```
Input: board = [ [ "X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X" ] ]

Output: [ [ "X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X" ] ]

Explanation:

In the above diagram, the bottom region is not captured because it is on the edge of the board and cannot be surrounded.
```

**Example 2**

```
Input: board = [ [ "X" ] ]

Output: [ [ "X" ] ]
```

## Method 1

```tex
【O(m * n) time | O(m * n) space】
```

```java
package Leetcode.Graphs;

/**
 * @Author zhengxingxing
 * @Date 2025/06/29
 */
public class SurroundedRegions {
    
    public static void solve(char[][] board) {
        // Get the number of rows and columns in the board
        int m = board.length, n = board[0].length;

        // Step 1: Mark all 'O's that are connected to the boundary with a temporary marker '#'
        // These 'O's cannot be captured because they are not fully surrounded by 'X'

        // Traverse each row's first and last column (left and right boundaries)
        for (int i = 0; i < m; i++) {
            dfs(board, i, 0, m, n);         // Left boundary
            dfs(board, i, n - 1, m, n);     // Right boundary
        }

        // Traverse each column's first and last row (top and bottom boundaries)
        for (int j = 0; j < n; j++) {
            dfs(board, 0, j, m, n);         // Top boundary
            dfs(board, m - 1, j, m, n);     // Bottom boundary
        }

        // Step 2: Flip all remaining 'O's to 'X' (these are surrounded regions)
        // Restore all '#' back to 'O' (these were connected to the boundary)
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // If the cell is still 'O', it means it is surrounded and should be captured
                if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                    // If the cell is '#', it was connected to the boundary and should be restored
                } else if (board[i][j] == '#') {
                    board[i][j] = 'O';
                }
            }
        }
    }
    
    private static void dfs(char[][] board, int row, int col, int m, int n) {
        // If out of bounds or not an 'O', stop recursion
        if (row < 0 || row >= m || col < 0 || col >= n || board[row][col] != 'O') {
            return;
        }
        // Mark the current cell as '#' to indicate it is connected to the boundary
        board[row][col] = '#';
        // Recursively mark all adjacent 'O's (down, up, right, left)
        dfs(board, row + 1, col, m, n); // Down
        dfs(board, row - 1, col, m, n); // Up
        dfs(board, row, col + 1, m, n); // Right
        dfs(board, row, col - 1, m, n); // Left
    }

    public static void main(String[] args) {
        char[][] board1 = {
                {'X','X','X','X'},
                {'X','O','O','X'},
                {'X','X','O','X'},
                {'X','O','X','X'}
        };
        solve(board1);
        printBoard(board1); // OutPut: [ [ "X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X" ] ]

        char[][] board2 = {
                {'X'}
        };
        solve(board2);
        printBoard(board2); // OutPut: [ [ "X" ] ]
    }
    
    private static void printBoard(char[][] board) {
        for (char[] row : board) {
            System.out.println(java.util.Arrays.toString(row));
        }
        System.out.println();
    }
}

```





