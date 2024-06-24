---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "36.Valid Sudoku"
date: "2024-02-08"
categories:
  - "LeetCode Array"
---

# LeetCode 36. Valid Sudoku [Easy]

- Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

  1. Each row must contain the digits `1-9` without repetition.
  2. Each column must contain the digits `1-9` without repetition.
  3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

  **Note:**

  - A Sudoku board (partially filled) could be valid but is not necessarily solvable.
  - Only the filled cells need to be validated according to the mentioned rules.

**Example 1**

```
Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: true
```

**Example 2**

```
Input: board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

## Method 1

```tex
【O(1)time∣O(1)space】
```

```java
package Leetcode.Array;

/**
 * @author zhengstars
 * @date 2024/03/09
 */
public class ValidSudoku {
    public static boolean isValidSudoku(char[][] board) {
        // Initialize data structure
        int[][] rows = new int[9][9];
        int[][] columns = new int[9][9];
        int[][][] subboxes = new int[3][3][9];

        // Traverse the board
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char c = board[i][j];

                // If this cell is empty, continue to the next one
                if (c != '.') {
                    int index = c - '0' - 1;

                    // Increment counts for this number in the row, column and subbox
                    rows[i][index]++;
                    columns[j][index]++;
                    subboxes[i / 3][j / 3][index]++;

                    // If this number has already been seen before in the current row, column or subbox, return false
                    if (rows[i][index] > 1 || columns[j][index] > 1 || subboxes[i / 3][j / 3][index] > 1) {
                        return false;
                    }
                }
            }
        }

        // If no duplicate is found, return true
        return true;
    }

    public static void main(String[] args) {

        char[][] test1 = {
                {'5', '3', '.', '.', '7', '.', '.', '.', '.'},
                {'6', '.', '.', '1', '9', '5', '.', '.', '.'},
                {'.', '9', '8', '.', '.', '.', '.', '6', '.'},
                {'8', '.', '.', '.', '6', '.', '.', '.', '3'},
                {'4', '.', '.', '8', '.', '3', '.', '.', '1'},
                {'7', '.', '.', '.', '2', '.', '.', '.', '6'},
                {'.', '6', '.', '.', '.', '.', '2', '8', '.'},
                {'.', '.', '.', '4', '1', '9', '.', '.', '5'},
                {'.', '.', '.', '.', '8', '.', '.', '7', '9'}
        };
        System.out.println(isValidSudoku(test1));  // 输出: true

        char[][] test2 = {
                {'8', '3', '.', '.', '7', '.', '.', '.', '.'},
                {'6', '.', '.', '1', '9', '5', '.', '.', '.'},
                {'.', '9', '8', '.', '.', '.', '.', '6', '.'},
                {'8', '.', '.', '.', '6', '.', '.', '.', '3'},
                {'4', '.', '.', '8', '.', '3', '.', '.', '1'},
                {'7', '.', '.', '.', '2', '.', '.', '.', '6'},
                {'.', '6', '.', '.', '.', '.', '2', '8', '.'},
                {'.', '.', '.', '4', '1', '9', '.', '.', '5'},
                {'.', '.', '.', '.', '8', '.', '.', '7', '9'}
        };
        System.out.println(isValidSudoku(test2));  // 输出: false
    }
}

```

