---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "695. Max Area of Island"
date: "2025-06-26"
tags: Medium
categories:
  - "LeetCode Graphs"
---


- You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.
- The **area** of an island is the number of cells with a value `1` in the island.
- Return *the maximum **area** of an island in* `grid`. If there is no island, return `0`.

**Example 1**

```
Input: grid = [ [ 0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0 ] ]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.
```

**Example 2**

```
Input: grid = [ [ 0,0,0,0,0,0,0,0 ] ]
Output: 0
```

## Method 1

```tex
【O(m * n) time | O(m * n) space】
```

```java
package Leetcode.Graphs;

/**
 * @Author zhengxingxing
 * @Date 2025/06/26
 */
public class MaxAreaOfIsland {

    public static int maxAreaOfIsland(int[][] grid) {
        int maxArea = 0; // To keep track of the largest island area found
        int m = grid.length; // Number of rows in the grid
        int n = grid[0].length; // Number of columns in the grid

        // Traverse every cell in the grid
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // If the current cell is land (1), start a DFS to calculate the area of this island
                if (grid[i][j] == 1){
                    // Update maxArea if the current island's area is larger
                    maxArea = Math.max(maxArea, dfs(grid, i, j));
                }
            }
        }
        return maxArea;
    }
    
    private static int dfs(int[][] grid, int i, int j) {
        int m = grid.length; // Number of rows
        int n = grid[0].length; // Number of columns

        // If out of bounds or the cell is water (0), return 0 (no area)
        if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == 0){
            return 0;
        }

        // Mark the current cell as visited by setting it to 0 (water)
        grid[i][j] = 0;
        int area = 1; // Current cell counts as 1 area

        // Recursively explore all four directions and sum up the area
        area += dfs(grid, i + 1, j); // Down
        area += dfs(grid, i - 1, j); // Up
        area += dfs(grid, i, j + 1); // Right
        area += dfs(grid, i, j - 1); // Left

        return area;
    }

    /**
     * Main method to test the maxAreaOfIsland function with sample test cases.
     */
    public static void main(String[] args) {
        int[][] grid1 = {
                {0,0,1,0,0,0,0,1,0,0,0,0,0},
                {0,0,0,0,0,0,0,1,1,1,0,0,0},
                {0,1,1,0,1,0,0,0,0,0,0,0,0},
                {0,1,0,0,1,1,0,0,1,0,1,0,0},
                {0,1,0,0,1,1,0,0,1,1,1,0,0},
                {0,0,0,0,0,0,0,0,0,0,1,0,0},
                {0,0,0,0,0,0,0,1,1,1,0,0,0},
                {0,0,0,0,0,0,0,1,1,0,0,0,0}
        };
        int[][] grid2 = {
                {0,0,0,0,0,0,0,0}
        };
        // Output should be 6 for grid1 and 0 for grid2
        System.out.println(maxAreaOfIsland(grid1)); // Output: 6
        System.out.println(maxAreaOfIsland(grid2)); // Output: 0
    }
}

```





