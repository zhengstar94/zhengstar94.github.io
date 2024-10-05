---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "62.Unique Paths"
date: "2024-10-01"
categories:
  - "LeetCode Dynamic Programming"
---

- There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

  Given the two integers `m` and `n`, return *the number of possible unique paths that the robot can take to reach the bottom-right corner*.

  The test cases are generated so that the answer will be less than or equal to `2 * 10^9`.

**Example 1**

```
Input: m = 3, n = 7
Output: 28
```

**Example 2**

```
Input: m = 3, n = 2
Output: 3
Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down
```

**Example 3**

```
Input: m = 3, n = 3
Output: 6
```

## Method 1

```tex
【O(m * n) time | O(n) space】
```

```java
package Leetcode.DynamicProgramming;

import java.util.Arrays;

/**
 * Solution for LeetCode 62: Unique Paths
 * @author zhengstars
 * @date 2024/10/01
 */
public class UniquePaths {
    /**
     * Calculates the number of unique paths from top-left to bottom-right corner of a m x n grid.
     *
     * @param m Number of rows in the grid
     * @param n Number of columns in the grid
     * @return The total number of unique paths
     */
    public int uniquePaths(int m, int n) {
        // Initialize a 1D array to store the number of paths for each column
        // The size is n because we only need to keep track of one row at a time
        int[] dp = new int[n];

        // Fill the array with 1s initially
        // This represents the first row where there's only one way to reach each cell
        Arrays.fill(dp, 1);

        // Iterate through each row (starting from the second row)
        for (int i = 1; i < m; i++) {
            // For each column (starting from the second column)
            for (int j = 1; j < n; j++) {
                // Update the number of paths to the current cell
                // It's the sum of paths from the cell above (current dp[j])
                // and the cell to the left (dp[j-1])
                dp[j] += dp[j - 1];
            }
        }

        // The last element of dp array contains the total number of unique paths
        // to reach the bottom-right corner
        return dp[n - 1];
    }

    /**
     * Main method for testing the uniquePaths function
     */
    public static void main(String[] args) {
        UniquePaths solution = new UniquePaths();

        // Test case 1: 3x7 grid
        System.out.println("3x7 grid: " + solution.uniquePaths(3, 7)); // Expected output: 28

        // Test case 2: 3x2 grid
        System.out.println("3x2 grid: " + solution.uniquePaths(3, 2)); // Expected output: 3

        // Test case 3: 7x3 grid
        System.out.println("7x3 grid: " + solution.uniquePaths(7, 3)); // Expected output: 28

        // Test case 4: 3x3 grid
        System.out.println("3x3 grid: " + solution.uniquePaths(3, 3)); // Expected output: 6

        // Test case 5: 1x1 grid
        System.out.println("1x1 grid: " + solution.uniquePaths(1, 1)); // Expected output: 1

        // Test case 6: 10x10 grid
        System.out.println("10x10 grid: " + solution.uniquePaths(10, 10)); // Expected output: 48620
    }
}
```





