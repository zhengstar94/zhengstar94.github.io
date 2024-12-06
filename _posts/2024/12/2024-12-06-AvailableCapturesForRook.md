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
Input: board = [ [ ".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","R",".",".",".","p"],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","." ] ]

Output: 3

Explanation:

In this example, the rook is attacking all the pawns.
```

**Example 2**

```
Input: board = [ [ ".",".",".",".",".",".","."],[".","p","p","p","p","p",".","."],[".","p","p","B","p","p",".","."],[".","p","B","R","B","p",".","."],[".","p","p","B","p","p",".","."],[".","p","p","p","p","p",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","." ] ]

Output: 0

Explanation:

The bishops are blocking the rook from attacking any of the pawns.
```

**Example 3**

```
Input: board = [ [ ".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","p",".",".",".","."],["p","p",".","R",".","p","B","."],[".",".",".",".",".",".",".","."],[".",".",".","B",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","." ] ]

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
    int captures = 0; // Counter for captured pawns
    int rookRow = -1, rookCol = -1; // Initial rook position coordinates

    // Find the rook's position on the chessboard
    // Nested loops iterate through each square to locate the rook
    for (int i = 0; i < 8; i++) {
      for (int j = 0; j < 8; j++) {
        if (board[i][j] == 'R') {
          // When rook is found, store its row and column coordinates
          rookRow = i;
          rookCol = j;
          break;
        }
      }
      // If the rook's row has been found (rookRow is no longer -1), 
      // immediately exit the outer loop to optimize the search process. 
      // This prevents unnecessary iterations through the remaining rows 
      // after the rook's position has already been located, 
      // improving the time efficiency of finding the rook on the chessboard.
      if (rookRow != -1){
        break;
      }
    }

    // Check capture possibilities in four directions from rook's position

    // Upward direction capture check
    for (int i = rookRow - 1; i >= 0; i--) {
      if (board[i][rookCol] == 'B') {
        break; // Stop if a bishop blocks the path
      }
      if (board[i][rookCol] == 'p') {
        captures++; // Capture the pawn
        break; // Stop checking this direction after capturing
      }
    }

    // Downward direction capture check
    for (int i = rookRow + 1; i < 8; i++) {
      if (board[i][rookCol] == 'B') {
        break; // Stop if a bishop blocks the path
      }
      if (board[i][rookCol] == 'p') {
        captures++; // Capture the pawn
        break; // Stop checking this direction after capturing
      }
    }

    // Left direction capture check
    for (int j = rookCol - 1; j >= 0; j--) {
      if (board[rookRow][j] == 'B') {
        break; // Stop if a bishop blocks the path
      }
      if (board[rookRow][j] == 'p') {
        captures++; // Capture the pawn
        break; // Stop checking this direction after capturing
      }
    }

    // Right direction capture check
    for (int j = rookCol + 1; j < 8; j++) {
      if (board[rookRow][j] == 'B'){
        break; // Stop if a bishop blocks the path
      }
      if (board[rookRow][j] == 'p') {
        captures++; // Capture the pawn
        break; // Stop checking this direction after capturing
      }
    }

    return captures; // Return total number of captured pawns
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






