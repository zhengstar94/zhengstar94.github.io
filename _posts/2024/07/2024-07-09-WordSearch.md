---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "79.Word Search"
date: "2024-07-09"
categories:
  - "LeetCode Backtracking"
---

# 79. Word Search

- Given an `m x n` grid of characters `board` and a string `word`, return `true` *if* `word` *exists in the grid*.
- The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1**

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
```

**Example 2**

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true
```

**Example 3**

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
```

## Method 1

```tex
【O(4^k * m * n) time | O(k) space】
```

```java
package Leetcode.Backtracking;

/**
 * This class implements the Word Search problem using backtracking algorithm.
 *
 * Given an m x n grid of characters board and a string word, this algorithm returns true if word exists in the grid.
 * The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring.
 * The same letter cell may not be used more than once.
 *
 * @author zhengstars
 * @date 2024/07/09
 */
public class WordSearch {

    /**
     * Check if the given word exists in the board.
     *
     * @param board the character grid
     * @param word the target word
     * @return true if the word exists in the grid, false otherwise
     */
    public static boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;

        // Iterate through each cell in the board
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (dfs(board, word, i, j, 0)) {
                    return true;
                }
            }
        }
        return false;
    }

    /**
     * Depth-first search to find the word in the board.
     *
     * @param board the character grid
     * @param word the target word
     * @param i the row index of the current cell
     * @param j the column index of the current cell
     * @param k the index of the current character in the word
     * @return true if the word is found, false otherwise
     */
    private static boolean dfs(char[][] board, String word, int i, int j, int k) {
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || board[i][j] != word.charAt(k)) {
            return false;
        }

        // Check if the end of the word is reached
        if (k == word.length() - 1) {
            return true;
        }

        char temp = board[i][j];
        board[i][j] = ' '; // Mark as visited

        // Explore adjacent cells recursively
        boolean result = dfs(board, word, i + 1, j, k + 1) ||
                dfs(board, word, i - 1, j, k + 1) ||
                dfs(board, word, i, j + 1, k + 1) ||
                dfs(board, word, i, j - 1, k + 1);

        board[i][j] = temp; // Restore original value
        return result;
    }

    /**
     * Main method to test the Word Search algorithm.
     */
    public static void main(String[] args) {
        char[][] board = {
                {'A','B','C','E'},
                {'S','F','C','S'},
                {'A','D','E','E'}
        };

        String word1 = "ABCCED";
        String word2 = "SEE";
        String word3 = "ABCB";

        System.out.println(exist(board, word1)); // true
        System.out.println(exist(board, word2)); // true
        System.out.println(exist(board, word3)); // false
    }
}
```

