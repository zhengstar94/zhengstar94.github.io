---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1254. Number of Closed Islands"
date: "2025-06-29"
tags: Medium
categories:
  - "LeetCode Graphs"
---


- Given a 2D `grid` consists of `0s` (land) and `1s` (water). An *island* is a maximal 4-directionally connected group of `0s` and a *closed island* is an island **totally** (all left, top, right, bottom) surrounded by `1s.`
- Return the number of *closed islands*.

**Example 1**

```
Input: grid = [ [ 1,1,1,1,1,1,1,0],[1,0,0,0,0,1,1,0],[1,0,1,0,1,1,1,0],[1,0,0,0,0,1,0,1],[1,1,1,1,1,1,1,0 ] ]
Output: 2
Explanation: 
Islands in gray are closed because they are completely surrounded by water (group of 1s).
```

**Example 2**

```
Input: grid = [ [ 0,0,1,0,0],[0,1,0,1,0],[0,1,1,1,0 ] ]
Output: 1
```

**Example 3**

```
Input: grid = [ [ 1,1,1,1,1,1,1],
               [1,0,0,0,0,0,1],
               [1,0,1,1,1,0,1],
               [1,0,1,0,1,0,1],
               [1,0,1,1,1,0,1],
               [1,0,0,0,0,0,1],
               [1,1,1,1,1,1,1 ] ]
Output: 2
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
public class NumberOfClosedIslands {
    
    public static int closedIsland(int[][] grid) {
        // Get the number of rows and columns in the grid
        int m = grid.length;
        int n = grid[0].length;

        // Step 1: Flood-fill all land cells (0s) that are connected to the grid's boundary.
        // These cannot be closed islands because they touch the edge.
        // Flood-fill leftmost and rightmost columns for every row
        for (int i = 0; i < m; i++) {
            dfs(grid, i, 0, m, n);         // Left boundary
            dfs(grid, i, n - 1, m, n);     // Right boundary
        }

        // Flood-fill topmost and bottommost rows for every column
        for (int j = 0; j < n; j++) {
            dfs(grid, 0, j, m, n);         // Top boundary
            dfs(grid, m - 1, j, m, n);     // Bottom boundary
        }

        // Step 2: Traverse the inner grid to count closed islands
        int count = 0;
        // Only check cells that are not on the boundary
        for (int i = 1; i < m - 1; i++) {
            for (int j = 1; j < n - 1; j++) {
                // If a land cell (0) is found, it must be a closed island
                if (grid[i][j] == 0) {
                    // Use DFS to flood-fill the entire island, marking it as visited
                    dfs(grid, i, j, m, n);
                    // Increment the closed island count
                    count++;
                }
            }
        }
        // Return the total number of closed islands found
        return count;
    }
    
    private static void dfs(int[][] grid, int row, int col, int m, int n) {
        // If out of bounds or not a land cell, stop recursion
        if (row < 0 || row >= m || col < 0 || col >= n || grid[row][col] != 0) {
            return;
        }
        // Mark the current land cell as visited by setting it to 1 (water)
        grid[row][col] = 1;
        // Recursively flood-fill all four directions (down, up, right, left)
        dfs(grid, row + 1, col, m, n); // Down
        dfs(grid, row - 1, col, m, n); // Up
        dfs(grid, row, col + 1, m, n); // Right
        dfs(grid, row, col - 1, m, n); // Left
    }
    
    public static void main(String[] args) {
        // Test case 1: Two closed islands
        int[][] grid1 = {
                {1,1,1,1,1,1,1,0},
                {1,0,0,0,0,1,1,0},
                {1,0,1,0,1,1,1,0},
                {1,0,0,0,0,1,0,1},
                {1,1,1,1,1,1,1,0}
        };
        // Test case 2: One closed island
        int[][] grid2 = {
                {0,0,1,0,0},
                {0,1,0,1,0},
                {0,1,1,1,0}
        };
        // Test case 3: Two closed islands
        int[][] grid3 = {
                {1,1,1,1,1,1,1},
                {1,0,0,0,0,0,1},
                {1,0,1,1,1,0,1},
                {1,0,1,0,1,0,1},
                {1,0,1,1,1,0,1},
                {1,0,0,0,0,0,1},
                {1,1,1,1,1,1,1}
        };
        // Output the results for each test case
        System.out.println(closedIsland(grid1)); // Output: 2
        System.out.println(closedIsland(grid2)); // Output: 1
        System.out.println(closedIsland(grid3)); // Output: 2
    }
}

```





