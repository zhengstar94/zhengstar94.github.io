---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "417.Pacific Atlantic Water Flow"
date: "2024-07-12"
categories:
  - "LeetCode Graphs"
---

# 417. Pacific Atlantic Water Flow

- There is an `m x n` rectangular island that borders both the **Pacific Ocean** and **Atlantic Ocean**. The **Pacific Ocean** touches the island's left and top edges, and the **Atlantic Ocean** touches the island's right and bottom edges.
- The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix `heights` where `heights[r][c]` represents the **height above sea level** of the cell at coordinate `(r, c)`.
- The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is **less than or equal to** the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.
- Return *a **2D list** of grid coordinates* `result` *where* `result[i] = [ri, ci]` *denotes that rain water can flow from cell* `(ri, ci)` *to **both** the Pacific and Atlantic oceans*.

**Example 1**

```
Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
Explanation: The following cells can flow to the Pacific and Atlantic oceans, as shown below:
[0,4]: [0,4] -> Pacific Ocean 
       [0,4] -> Atlantic Ocean
[1,3]: [1,3] -> [0,3] -> Pacific Ocean 
       [1,3] -> [1,4] -> Atlantic Ocean
[1,4]: [1,4] -> [1,3] -> [0,3] -> Pacific Ocean 
       [1,4] -> Atlantic Ocean
[2,2]: [2,2] -> [1,2] -> [0,2] -> Pacific Ocean 
       [2,2] -> [2,3] -> [2,4] -> Atlantic Ocean
[3,0]: [3,0] -> Pacific Ocean 
       [3,0] -> [4,0] -> Atlantic Ocean
[3,1]: [3,1] -> [3,0] -> Pacific Ocean 
       [3,1] -> [4,1] -> Atlantic Ocean
[4,0]: [4,0] -> Pacific Ocean 
       [4,0] -> Atlantic Ocean
Note that there are other possible paths for these cells to flow to the Pacific and Atlantic oceans.
```

**Example 2**

```
Input: heights = [ [1]]
Output: [ [0,0]]
Explanation: The water can flow from the only cell to the Pacific and Atlantic oceans.
```

## Method 1

```tex
【O(n*m) time | O(n*m) space】
```

```java
package Leetcode.Graphs;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengstars
 * @date 2024/07/12
 */
public class PacificAtlanticWaterFlow {

    /**
     * Main function to perform the water flow problem for the Pacific and Atlantic oceans.
     * @param heights the input matrix representing the heights of the land
     * @return a 2D list of grid coordinates where water can flow to both oceans
     */
    public static List<List<Integer>> pacificAtlantic(int[][] heights) {
        List<List<Integer>> result = new ArrayList<>();

        if (heights == null || heights.length == 0 || heights[0].length == 0) {
            return result;
        }

        int m = heights.length;
        int n = heights[0].length;

        boolean[][] canReachPacific = new boolean[m][n];
        boolean[][] canReachAtlantic = new boolean[m][n];

        // Start DFS from the left and right boundaries to mark reachable cells for Pacific and Atlantic
        for (int i = 0; i < m; i++) {
            dfs(heights, canReachPacific, i, 0);
            dfs(heights, canReachAtlantic, i, n - 1);
        }

        // Start DFS from the top and bottom boundaries to mark reachable cells for Pacific and Atlantic
        for (int j = 0; j < n; j++) {
            dfs(heights, canReachPacific, 0, j);
            dfs(heights, canReachAtlantic, m - 1, j);
        }

        // Check for cells that can reach both oceans and add them to the result list
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (canReachPacific[i][j] && canReachAtlantic[i][j]) {
                    List<Integer> cell = new ArrayList<>();
                    cell.add(i);
                    cell.add(j);
                    result.add(cell);
                }
            }
        }

        return result;
    }

    /**
     * Depth-first search to mark reachable cells from a given cell (x, y) in the matrix heights.
     * @param heights the input matrix representing the heights of the land
     * @param canReach boolean matrix to mark cells that can be reached from a given cell
     * @param x the row index of the current cell
     * @param y the column index of the current cell
     */
    private static void dfs(int[][] heights, boolean[][] canReach, int x, int y) {
        if (canReach[x][y]) {
            return;
        }

        int m = heights.length;
        int n = heights[0].length;

        canReach[x][y] = true;

        int[][] directions = { {1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        for (int[] direction : directions) {
            int newX = x + direction[0];
            int newY = y + direction[1];

            if (newX >= 0 && newX < m && newY >= 0 && newY < n && heights[newX][newY] >= heights[x][y]) {
                dfs(heights, canReach, newX, newY);
            }
        }
    }

    /**
     * Main method to test the solution.
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        int[][] heights = { {1, 2, 2, 3, 5}, {3, 2, 3, 4, 4}, {2, 4, 5, 3, 1}, {6, 7, 1, 4, 5}, {5, 1, 1, 2, 4}};

        List<List<Integer>> result = pacificAtlantic(heights);

        System.out.println("Output: " + result);
    }
}
```

