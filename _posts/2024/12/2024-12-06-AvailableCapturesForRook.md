---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "999. Available Captures for Rook"
date: "2024-12-06"
tags: Easy
categories:
  - "LeetCode Math"
---

- You are given an `8 x 8` **matrix** representing a chessboard. There is **exactly one** white rook represented by `'R'`, some number of white bishops `'B'`, and some number of black pawns `'p'`. Empty squares are represented by `'.'`.
- A rook can move any number of squares horizontally or vertically (up, down, left, right) until it reaches another piece *or* the edge of the board. A rook is **attacking** a pawn if it can move to the pawn's square in one move.
- Note: A rook cannot move through other pieces, such as bishops or pawns. This means a rook cannot attack a pawn if there is another piece blocking the path.
- Return the **number of pawns** the white rook is **attacking**.

**Example 1**

```
Input: board = [[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","R",".",".",".","p"],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]

Output: 3

Explanation:

In this example, the rook is attacking all the pawns.
```

**Example 2**

```
Input: board = [[".",".",".",".",".",".","."],[".","p","p","p","p","p",".","."],[".","p","p","B","p","p",".","."],[".","p","B","R","B","p",".","."],[".","p","p","B","p","p",".","."],[".","p","p","p","p","p",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]

Output: 0

Explanation:

The bishops are blocking the rook from attacking any of the pawns.
```

**Example 3**

```
Input: board = [[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","p",".",".",".","."],["p","p",".","R",".","p","B","."],[".",".",".",".",".",".",".","."],[".",".",".","B",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."]]

Output: 3

Explanation:

The rook is attacking the pawns at positions b5, d6, and f5.
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2024/12/06
 */
public class AvailableCapturesForRook {
    public static int numRookCaptures(char[][] board) {
        // Initialize the row and column of the rook to -1
        // These variables will store the exact location of the white rook on the board
        int rRow = -1, rCol = -1;

        // Nested loops to find the rook's position on the 8x8 board
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 8; j++) {
                // When the rook ('R') is found, store its row and column
                if (board[i][j] == 'R') {
                    rRow = i;
                    rCol = j;
                    break; // Exit inner loop once rook is found
                }
            }

            // CRITICAL DETAILED EXPLANATION OF THIS LINE:
            // After finding the rook in the inner loop, this condition breaks the outer loop
            // Prevents unnecessary iterations once the rook is located
            // - If rRow is not -1, it means the rook has been found and its position recorded
            // - This is an optimization to exit both loops as soon as the rook is located
            // - Without this, the loop would continue checking remaining board rows even after finding the rook
            // - Reduces unnecessary computations by early termination
            if (rRow != -1) {
                break;
            }
        }

        // Define movement directions: Up, Down, Left, Right
        // Each direction represented as [row_change, column_change]
        int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

        // Counter to track the number of pawns the rook can capture
        int captures = 0;

        // Explore each of the four directions from the rook's position
        for (int[] dir : directions) {
            // Initialize search starting point in current direction
            int row = rRow + dir[0];
            int col = rCol + dir[1];

            // Continue searching while within board boundaries
            while (row >= 0 && row < 8 && col >= 0 && col < 8) {
                // If a bishop is encountered, this direction is blocked
                // Stop searching in this direction
                if (board[row][col] == 'B') {
                    break;
                }

                // If a pawn is found, increment capture count
                // Stop searching in this direction after capturing
                if (board[row][col] == 'p') {
                    captures++;
                    break;
                }

                // Move to next square in current direction
                row += dir[0];
                col += dir[1];
            }
        }

        return captures;
    }


    public static void main(String[] args) {
        // Test Case 1: Pawns available in multiple directions
        char[][] board1 = {
                {'.','.','.','.','.','.','.','.',},
                {'.','.','.','.','.','p','.','.'},
                {'.','.','.','.','.','R','.','.'},
                {'.','.','.','.','.','.','.','.',},
                {'.','.','.','p','.','.','.','.'},
                {'.','.','.','.','.','.','.','.',},
                {'.','.','.','.','.','.','.','.',},
                {'.','.','.','.','.','.','.','.',}
        };
        System.out.println("Test Case 1 Result: " + numRookCaptures(board1));

        // Test Case 2: Bishops blocking all pawns
        char[][] board2 = {
                {'.','.','.','.','.','.','.','.',},
                {'.','p','p','p','p','p','.','p'},
                {'.','p','B','R','B','p','.','p'},
                {'.','p','p','p','p','p','.','p'},
                {'.','.','.','.','.','.','.','.',},
                {'.','.','.','.','.','.','.','.',},
                {'.','.','.','.','.','.','.','.',},
                {'.','.','.','.','.','.','.','.',}
        };
        System.out.println("Test Case 2 Result: " + numRookCaptures(board2));

        // Test Case 3: Pawns available in some directions
        char[][] board3 = {
                {'.','.','.','.','.','.','.','.',},
                {'.','.','.','.','.','p','.','.'},
                {'.','.','.','B','R','p','.','.',},
                {'.','.','.','.','.','.','.','.',},
                {'.','.','.','.','.','.','.','.',},
                {'.','.','.','.','.','.','.','.',},
                {'.','.','.','.','.','.','.','.',},
                {'.','.','.','.','.','.','.','.',}
        };
        System.out.println("Test Case 3 Result: " + numRookCaptures(board3));
    }
}

```






