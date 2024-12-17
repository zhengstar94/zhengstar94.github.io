---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "885. Spiral Matrix III"
date: "2024-12-17"
tags: Medium
categories:
  - "LeetCode Array"
---


- You start at the cell `(rStart, cStart)` of an `rows x cols` grid facing east. The northwest corner is at the first row and column in the grid, and the southeast corner is at the last row and column.
- You will walk in a clockwise spiral shape to visit every position in this grid. Whenever you move outside the grid's boundary, we continue our walk outside the grid (but may return to the grid boundary later.). Eventually, we reach all `rows * cols` spaces of the grid.
- Return *an array of coordinates representing the positions of the grid in the order you visited them*.


**Example 1**

```
Input: rows = 1, cols = 4, rStart = 0, cStart = 0
Output: [ [ 0,0],[0,1],[0,2],[0,3 ] ]
```

**Example 2**

```
Input: rows = 5, cols = 6, rStart = 1, cStart = 4
Output: [ [ 1,4],[1,5],[2,5],[2,4],[2,3],[1,3],[0,3],[0,4],[0,5],[3,5],[3,4],[3,3],[3,2],[2,2],[1,2],[0,2],[4,5],[4,4],[4,3],[4,2],[4,1],[3,1],[2,1],[1,1],[0,1],[4,0],[3,0],[2,0],[1,0],[0,0 ] ]
```

## Method 1

```tex
【O(rows * cols) time | O(rows * cols) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2024/12/17
 */
public class SpiralMatrixIII {
    public static int[][] spiralMatrixIII(int rows, int cols, int rStart, int cStart) {
        // Initialize the result array to store the positions visited
        int[][] result = new int[rows * cols][2];
        // Initialize the count to track the number of visited cells
        int count = 0;
        // Start at the given initial position (rStart, cStart)
        result[count++] = new int[]{rStart, cStart};

        // Define the four directions in the order: Right -> Down -> Left -> Up
        int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        // Initialize step size for each direction
        int step = 1;
        // Initialize the direction index to point to the first direction (Right)
        int dirIndex = 0;

        // Loop until all cells are visited
        while (count < rows * cols) {
            // Each step size is repeated twice: first horizontally, then vertically
            for (int i = 0; i < 2; i++) { // Loop over horizontal and vertical moves
                for (int j = 0; j < step; j++) { // Move in the current direction 'step' times
                    // Update the row and column based on the current direction
                    rStart += directions[dirIndex][0]; // Row movement
                    cStart += directions[dirIndex][1]; // Column movement

                    // Check if the current position is within the grid bounds
                    if (rStart >= 0 && rStart < rows && cStart >= 0 && cStart < cols) {
                        // Add the valid position to the result array
                        result[count++] = new int[]{rStart, cStart};
                    }
                }
                // After completing one direction (either horizontal or vertical), change direction
                dirIndex = (dirIndex + 1) % 4; // Use modulo to cycle through the four directions (0 to 3)
            }
            // Increase the step size after completing a full cycle of two directions (horizontal + vertical)
            step++;
        }
        return result;
    }

    public static void main(String[] args) {
        // Test case 1: Single row, multiple columns
        int[][] result1 = spiralMatrixIII(1, 4, 0, 0);
        System.out.println("Test Case 1:");
        for (int[] pos : result1) {
            System.out.print("[" + pos[0] + "," + pos[1] + "] ");
        }
        System.out.println();

        // Test case 2: Multiple rows and columns
        int[][] result2 = spiralMatrixIII(5, 6, 1, 4);
        System.out.println("Test Case 2:");
        for (int[] pos : result2) {
            System.out.print("[" + pos[0] + "," + pos[1] + "] ");
        }
        System.out.println();
    }
}

```





