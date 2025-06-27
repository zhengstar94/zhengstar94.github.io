---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "463. Island Perimeter"
date: "2025-06-27"
tags: Easy
categories:
  - "LeetCode Graphs"
---


- You are given `row x col` `grid` representing a map where `grid[i][j] = 1` represents land and `grid[i][j] = 0` represents water.
- Grid cells are connected **horizontally/vertically** (not diagonally). The `grid` is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).
- The island doesn't have "lakes", meaning the water inside isn't connected to the water around the island. One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

**Example 1**

```
Input: grid = [ [ 0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0 ] ]
Output: 16
Explanation: The perimeter is the 16 yellow stripes in the image above.
```

**Example 2**

```
Input: grid = [ [ 1 ] ]
Output: 4
```

**Example 3**

```
Input: grid = [ [ 1,0 ] ]
Output: 4
```

## Method 1

```tex
【O(m * n) time | O(1) space】
```

```java
package Leetcode.Graphs;

/**
 * @Author zhengxingxing
 * @Date 2025/06/27
 */
public class IslandPerimeter {

    public static int islandPerimeter(int[][] grid) {
        int m = grid.length;        // Number of rows in the grid
        int n = grid[0].length;     // Number of columns in the grid
        int perimeter = 0;          // Variable to store the total perimeter

        // Traverse each cell in the grid
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // If the current cell is land
                if(grid[i][j] == 1){
                    // Each land cell initially contributes 4 to the perimeter
                    perimeter += 4;

                    // If the cell above (i-1, j) is also land,
                    // it means the current cell shares a border with the upper cell.
                    // Each shared border should be subtracted twice (once for each cell),
                    // so we subtract 2 from the perimeter.
                    if (i > 0 && grid[i - 1][j] == 1){
                        perimeter -= 2;
                    }

                    // If the cell to the left (i, j-1) is also land,
                    // it means the current cell shares a border with the left cell.
                    // Again, subtract 2 for the shared border.
                    if (j > 0 && grid[i][j - 1] == 1) {
                        perimeter -= 2;
                    }
                }
            }
        }

        // Return the total calculated perimeter
        return perimeter;
    }

    public static void main(String[] args) {
        // Test case 1: Example from the problem description
        int[][] grid1 = {
                {0,1,0,0},
                {1,1,1,0},
                {0,1,0,0},
                {1,1,0,0}
        };
        System.out.println(islandPerimeter(grid1)); // Output: 16

        // Test case 2: Single land cell
        int[][] grid2 = {
                {1}
        };
        System.out.println(islandPerimeter(grid2)); // Output: 4

        // Test case 3: Two land cells in a row
        int[][] grid3 = {
                {1,0}
        };
        System.out.println(islandPerimeter(grid3)); // Output: 4
    }
}

```





