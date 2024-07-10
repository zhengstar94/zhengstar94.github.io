---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "200.Number of Islands"
date: "2024-07-10"
categories:
  - "LeetCode Graphs"
---

# 200. Number of Islands

- Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.
- An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1**

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

**Example 2**

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

## Method 1

```tex
【O(m * n) time | O(m * n) space】
```

```java
package Leetcode.Graphs;

/**
 * Calculates the number of islands in a 2D grid.
 *
 * @author zhengstars
 * @date 2024/07/10
 */
public class NumberOfIslands {
    /**
     * Main function to calculate the number of islands.
     * @param grid the 2D grid representing land and water
     * @return the number of islands
     */
    public static int numIslands(char[][] grid) {
        // Exception handling
        if (grid == null || grid.length == 0) {
            return 0;
        }

        int numIslands = 0;
        int rows = grid.length;
        int cols = grid[0].length;

        // Traverse the entire grid
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                // If encounter '1', it means finding a new island
                if (grid[i][j] == '1') {
                    numIslands++;
                    // Use DFS to mark all land of the current island
                    dfs(grid, i, j);
                }
            }
        }

        return numIslands;
    }

    /**
     * Depth-first search to mark visited land as '0'.
     * @param grid the 2D grid representing land and water
     * @param i the row index
     * @param j the column index
     */
    private static void dfs(char[][] grid, int i, int j) {
        // Exception handling: check boundary condition and whether the current grid is '1'
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] == '0') {
            return;
        }

        // Mark the current land as '0' to indicate visited
        grid[i][j] = '0';

        // Recursively process the adjacent positions of the current land
        dfs(grid, i - 1, j); // Up
        dfs(grid, i + 1, j); // Down
        dfs(grid, i, j - 1); // Left
        dfs(grid, i, j + 1); // Right
    }

    public static void main(String[] args) {

        char[][] grid1 = {
                { '1', '1', '1', '1', '0'},
                { '1', '1', '0', '1', '0'},
                { '1', '1', '0', '0', '0'},
                { '0', '0', '0', '0', '0'}
        };
        System.out.println(numIslands(grid1)); // Output: 1

        char[][] grid2 = {
                { '1', '1', '0', '0', '0'},
                { '1', '1', '0', '0', '0'},
                { '0', '0', '1', '0', '0'},
                { '0', '0', '0', '1', '1'}
        };
        System.out.println(numIslands(grid2)); // Output: 3
    }
}
```

