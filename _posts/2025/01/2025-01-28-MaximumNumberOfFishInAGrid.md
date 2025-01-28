---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2658. Maximum Number of Fish in a Grid"
date: "2025-01-28"
tags: Medium
categories:
  - "LeetCode DFS"
---

- You are given a **0-indexed** 2D matrix `grid` of size `m x n`, where `(r, c)` represents:
  - A **land** cell if `grid[r][c] = 0`, or
  - A **water** cell containing `grid[r][c]` fish, if `grid[r][c] > 0`.
- A fisher can start at any **water** cell `(r, c)` and can do the following operations any number of times:
  - Catch all the fish at cell `(r, c)`, or
  - Move to any adjacent **water** cell.
- Return *the **maximum** number of fish the fisher can catch if he chooses his starting cell optimally, or* `0` if no water cell exists.
- An **adjacent** cell of the cell `(r, c)`, is one of the cells `(r, c + 1)`, `(r, c - 1)`, `(r + 1, c)` or `(r - 1, c)` if it exists.

**Example 1**

```
Input: grid = [ [ 0,2,1,0],[4,0,0,3],[1,0,0,4],[0,3,2,0 ] ]
Output: 7
Explanation: The fisher can start at cell (1,3) and collect 3 fish, then move to cell (2,3) and collect 4 fish.
```

**Example 2**

```
Input: grid = [ [ 1,0,0,0],[0,0,0,0],[0,0,0,0],[0,0,0,1 ] ]
Output: 1
Explanation: The fisher can start at cells (0,0) or (3,3) and collect a single fish. 
```

## Method 1

```tex
【O(m * n) time | O(m * n) space】
```

```java
package Leetcode.DFS;

/**
 * @author zhengxingxing
 * @date 2025/01/28
 */
public class MaximumNumberOfFishInAGrid {

    // Define four directional movements: up, down, left, right
    // Used for exploring adjacent cells in the grid
    private static final int[][] DIRECTIONS = { { -1, 0}, {1, 0}, {0, -1}, {0, 1 } };

    public static int findMaxFish(int[][] grid) {
        // Check for null or empty grid
        if(grid == null || grid.length == 0){
            return 0;
        }

        // Get grid dimensions
        int m = grid.length;
        int n = grid[0].length;
        int maxFish = 0;

        // Iterate through each cell in the grid
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // If current cell is a water cell (contains fish)
                if (grid[i][j] > 0){
                    // Start DFS from this cell and update maximum fish count
                    maxFish = Math.max(maxFish, dfs(grid, i, j));
                }
            }
        }
        return maxFish;
    }

    private static int dfs(int[][] grid, int row, int col) {
        // Check boundaries and if cell is already visited (0)
        if (row < 0 || row >= grid.length ||
                col < 0 || col >= grid[0].length ||
                grid[row][col] == 0){
            return 0;
        }

        // Store current cell's fish count
        int currentFish = grid[row][col];
        // Mark cell as visited by setting to 0
        grid[row][col] = 0;

        // Initialize total fish count with current cell's fish
        int totalFish = currentFish;
        // Explore all four directions
        for (int[] dir: DIRECTIONS) {
            int newRow = row + dir[0];
            int newCol = col + dir[1];
            // Add fish count from adjacent water cells
            totalFish += dfs(grid, newRow, newCol);
        }

        return totalFish;
    }


    public static void main(String[] args) {
        // Test Case 1: Normal grid with multiple water cells
        int[][] grid1 = {
                {0, 2, 1, 0},
                {4, 0, 0, 3},
                {1, 0, 0, 4},
                {0, 3, 2, 0}
        };
        System.out.println("Test Case 1 Result: " + findMaxFish(grid1)); // Expected output: 7

        // Test Case 2: Grid with isolated water cells
        int[][] grid2 = {
                {1, 0, 0, 0},
                {0, 0, 0, 0},
                {0, 0, 0, 0},
                {0, 0, 0, 1}
        };
        System.out.println("Test Case 2 Result: " + findMaxFish(grid2)); // Expected output: 1

        // Test Case 3: Empty grid
        int[][] grid3 = {};
        System.out.println("Test Case 3 Result: " + findMaxFish(grid3)); // Expected output: 0

        // Test Case 4: Grid with only land cells
        int[][] grid4 = {
                {0, 0},
                {0, 0}
        };
        System.out.println("Test Case 4 Result: " + findMaxFish(grid4)); // Expected output: 0
    }
}

```





