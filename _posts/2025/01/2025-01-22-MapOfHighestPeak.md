---
toc:
    sidebar: left
giscus_comments: true
layout: post
title: "1765. Map of Highest Peak"
date: "2025-01-22"
tags: Medium
categories:
  - "LeetCode BFS"
---


- You are given an integer matrix `isWater` of size `m x n` that represents a map of **land** and **water** cells.

    - If `isWater[i][j] == 0`, cell `(i, j)` is a **land** cell.
    - If `isWater[i][j] == 1`, cell `(i, j)` is a **water** cell.

- You must assign each cell a height in a way that follows these rules:

    - The height of each cell must be non-negative.
    - If the cell is a **water** cell, its height must be `0`.
    - Any two adjacent cells must have an absolute height difference of **at most** `1`. A cell is adjacent to another cell if the former is directly north, east, south, or west of the latter (i.e., their sides are touching).

- Find an assignment of heights such that the maximum height in the matrix is **maximized**.

  Return *an integer matrix* `height` *of size* `m x n` *where* `height[i][j]` *is cell* `(i, j)`*'s height. If there are multiple solutions, return **any** of them*.

**Example 1**

```
Input: isWater = [ [ 0,1],[0,0 ] ]
Output: [ [ 1,0],[2,1 ] ]
Explanation: The image shows the assigned heights of each cell.
The blue cell is the water cell, and the green cells are the land cells.
```

**Example 2**

```
Input: isWater = [ [ 0,0,1],[1,0,0],[0,0,0 ] ]
Output: [ [ 1,1,0],[0,1,1],[1,2,2 ] ]
Explanation: A height of 2 is the maximum possible height of any assignment.
Any height assignment that has a maximum height of 2 while still meeting the rules will also be accepted.
```

## Method 1

```tex
【O(n * m) time | O(n * m) space】
```

```java
package Leetcode.BFS;

import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Queue;

/**
 * @author zhengxingxing
 * @date 2025/01/22
 */
public class MapOfHighestPeak {
    public static int[][] highestPeak(int[][] isWater) {
        // Define directions for adjacent cells (up, down, left, right)
        int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

        // Initialize queue for BFS (Breadth-First Search)
        Queue<int[]> queue = new ArrayDeque<>();

        // Get matrix dimensions
        int m = isWater.length;    // Number of rows
        int n = isWater[0].length; // Number of columns

        // Initialize the height matrix
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (isWater[i][j] == 1) {
                    // Add water cells to queue as starting points
                    queue.offer(new int[]{i, j});
                    isWater[i][j] = 0; // Set water height to 0
                } else {
                    isWater[i][j] = -1; // Mark land cells as unvisited
                }
            }
        }

        // BFS to assign heights to land cells
        while (!queue.isEmpty()) {
            // Get current cell from queue
            int[] point = queue.poll();
            int x = point[0]; // Current row
            int y = point[1]; // Current column

            // Check all four adjacent cells
            for (int[] dir: directions) {
                // Calculate new coordinates
                int newX = x + dir[0];
                int newY = y + dir[1];

                // Validate new coordinates and check if cell is unvisited
                if (newX >= 0 && newX < m && newY >= 0 && newY < n && isWater[newX][newY] == -1) {
                    // Set height of new cell to current cell's height + 1
                    isWater[newX][newY] = isWater[x][y] + 1;
                    // Add new cell to queue for further processing
                    queue.offer(new int[]{newX, newY});
                }
            }
        }

        return isWater; // Return the completed height matrix
    }

    public static void printMatrix(int[][] matrix) {
        for (int[] row : matrix) {
            System.out.println(Arrays.toString(row));
        }
    }

    // 测试用例
    public static void main(String[] args) {
        // Test Case 1: Simple 2x2 matrix
        int[][] isWater1 = {
                {0, 1},
                {0, 0}
        };
        System.out.println("Test Case 1 Result:");
        printMatrix(highestPeak(isWater1));
        // Expected output:
        // [1, 0]
        // [2, 1]

        // Test Case 2: 3x3 matrix with multiple water cells
        int[][] isWater2 = {
                {0, 0, 1},
                {1, 0, 0},
                {0, 0, 0}
        };
        System.out.println("\nTest Case 2 Result:");
        printMatrix(highestPeak(isWater2));
        // Expected output:
        // [1, 1, 0]
        // [0, 1, 1]
        // [1, 2, 2]

        // Test Case 3: All water cells
        int[][] isWater4 = {
                {1, 1},
                {1, 1}
        };
        System.out.println("\nTest Case 3 Result:");
        printMatrix(highestPeak(isWater4));
        // Expected output:
        // [0, 0]
        // [0, 0]
    }

}

```





