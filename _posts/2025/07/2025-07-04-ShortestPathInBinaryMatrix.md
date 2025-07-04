---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1091. Shortest Path in Binary Matrix"
date: "2025-07-04"
tags: Medium
categories:
  - "LeetCode BFS"
---


- Given an `n x n` binary matrix `grid`, return *the length of the shortest **clear path** in the matrix*. If there is no clear path, return `-1`.
- A **clear path** in a binary matrix is a path from the **top-left** cell (i.e., `(0, 0)`) to the **bottom-right** cell (i.e., `(n - 1, n - 1)`) such that:
  - All the visited cells of the path are `0`.
  - All the adjacent cells of the path are **8-directionally** connected (i.e., they are different and they share an edge or a corner).
- The **length of a clear path** is the number of visited cells of this path.

**Example 1**

```
Input: grid = [ [ 0,1],[1,0 ] ]
Output: 2
```

**Example 2**

```
Input: grid = [ [ 0,0,0],[1,1,0],[1,1,0 ] ]
Output: 4
```

**Example 3**

```
Input: grid = [ [ 1,0,0],[1,1,0],[1,1,0 ] ]
Output: -1
```

## Method 1

```tex
【O(n²) time | O(n²) space】
```

```java
package Leetcode.BFS;

import java.util.LinkedList;
import java.util.Queue;

/**
 * @Author zhengxingxing
 * @Date 2025/07/04
 */
public class ShortestPathInBinaryMatrix {
    // Define the 8 possible directions for movement (up, down, left, right, and 4 diagonals)
    private static final int[][] DIRS = {
            {1, 0},   // down
            {-1, 0},  // up
            {0, 1},   // right
            {0, -1},  // left
            {-1, 1},  // up-right
            {-1, -1}, // up-left
            {1, 1},   // down-right
            {1, -1}   // down-left
    };
    
    public static int shortestPathBinaryMatrix(int[][] grid) {
        int n = grid.length;
        // Special case: if the start or end cell is blocked, there is no valid path
        if (grid[0][0] == 1 || grid[n - 1][n - 1] == 1) {
            return -1;
        }
        // Initialize the BFS queue. Each element is an array: [x, y, pathLength]
        Queue<int[]> queue = new LinkedList<>();
        // Start from the top-left cell (0,0) with a path length of 1
        queue.offer(new int[]{0, 0, 1});
        // Mark the starting cell as visited by setting it to 1
        grid[0][0] = 1;

        // Begin BFS traversal
        while (!queue.isEmpty()) {
            // Remove the front element from the queue for processing
            int[] cur = queue.poll();
            int x = cur[0], y = cur[1], len = cur[2];
            // If we've reached the bottom-right cell, return the current path length
            if (x == n - 1 && y == n - 1) {
                return len;
            }
            // Explore all 8 possible directions from the current cell
            for (int[] dir : DIRS) {
                int nx = x + dir[0], ny = y + dir[1];
                // Check if the new cell is within bounds and is open (not blocked or visited)
                if (nx >= 0 && nx < n && ny >= 0 && ny < n && grid[nx][ny] == 0) {
                    // Add the new cell to the queue with an incremented path length
                    queue.offer(new int[]{nx, ny, len + 1});
                    // Mark the new cell as visited to prevent revisiting
                    grid[nx][ny] = 1;
                }
            }
        }
        // If the queue is empty and we haven't reached the end, there is no valid path
        return -1;
    }
    
    public static void main(String[] args) {
        int[][] grid1 = { { 0,1},{1,0 } };
        int[][] grid2 = { { 0,0,0},{1,1,0},{1,1,0 } };
        int[][] grid3 = { { 1,0,0},{1,1,0},{1,1,0 } };
        System.out.println(shortestPathBinaryMatrix(copyGrid(grid1))); // Output: 2
        System.out.println(shortestPathBinaryMatrix(copyGrid(grid2))); // Output: 4
        System.out.println(shortestPathBinaryMatrix(copyGrid(grid3))); // Output: -1
    }

    private static int[][] copyGrid(int[][] grid) {
        int n = grid.length;
        int[][] copy = new int[n][n];
        for (int i = 0; i < n; i++) {
            System.arraycopy(grid[i], 0, copy[i], 0, n);
        }
        return copy;
    }
}

```





