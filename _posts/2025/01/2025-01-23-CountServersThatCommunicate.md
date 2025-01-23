---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1267. Count Servers that Communicate"
date: "2025-01-23"
tags: Medium
categories:
  - "LeetCode Math"
---


- You are given a map of a server center, represented as a `m * n` integer matrix `grid`, where 1 means that on that cell there is a server and 0 means that it is no server. Two servers are said to communicate if they are on the same row or on the same column.
- Return the number of servers that communicate with any other server.

**Example 1**

```
Input: grid = [ [ 1,0],[0,1 ] ]
Output: 0
Explanation: No servers can communicate with others.
```

**Example 2**

```
Input: grid = [ [ 1,0],[1,1 ] ]
Output: 3
Explanation: All three servers can communicate with at least one other server.
```

**Example 3**

```
Input: grid = [ [ 1,1,0,0],[0,0,1,0],[0,0,1,0],[0,0,0,1 ] ]
Output: 4
Explanation: The two servers  in the first row can communicate with each other. The two servers in the third column can communicate with each other. The server at right bottom corner can't communicate with any other server.
```

## Method 1

```tex
【O(n * m) time | O(n + m) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/01/23
 */
public class CountServersThatCommunicate {
    
    public static int countServers(int[][] grid) {
        // Handle edge cases for null or empty grid
        if (grid == null || grid.length == 0){
            return 0;
        }

        // Get grid dimensions
        int m = grid.length;     // Number of rows
        int n = grid[0].length;  // Number of columns

        // Arrays to store count of servers in each row and column
        int[] rowCount = new int[m];
        int[] colCount = new int[n];

        // First pass: Count servers in each row and column
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid [ i ][ j ] == 1){
                    rowCount[i]++; // Increment server count for current row
                    colCount[j]++; // Increment server count for current column
                }
            }
        }

        // Second pass: Count servers that can communicate
        int result = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // A server can communicate if there's another server in same row or column
                if (grid[i][j] == 1 && (rowCount[i] > 1 || colCount[j] > 1)) {
                    result++;
                }
            }
        }

        return result;
    }

    public static void main(String[] args) {
        // Test case 1: No servers can communicate
        int[][] grid1 = { { 1,0}, {0,1 } };
        System.out.println("Test case 1 result: " + countServers(grid1)); // Expected output: 0

        // Test case 2: All servers can communicate
        int[][] grid2 = { { 1,0}, {1,1 } };
        System.out.println("Test case 2 result: " + countServers(grid2)); // Expected output: 3

        // Test case 3: Some servers can communicate
        int[][] grid3 = {
                {1,1,0,0},
                {0,0,1,0},
                {0,0,1,0},
                {0,0,0,1}
        };
        System.out.println("Test case 3 result: " + countServers(grid3)); // Expected output: 4

        // Test case 4: Empty grid
        int[][] grid4 = { };
        System.out.println("Test case 4 result: " + countServers(grid4)); // Expected output: 0

        // Test case 5: Single server (cannot communicate)
        int[][] grid5 = { { 1 } };
        System.out.println("Test case 5 result: " + countServers(grid5)); // Expected output: 0
    }
}

```





