---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3274. Check if Two Chessboard Squares Have the Same Color"
date: "2024-12-03"
tags: Easy
categories:
  - "LeetCode Math"
---

- You are given two strings, `coordinate1` and `coordinate2`, representing the coordinates of a square on an `8 x 8` chessboard.
- Return `true` if these two squares have the same color and `false` otherwise.
- The coordinate will always represent a valid chessboard square. The coordinate will always have the letter first (indicating its column), and the number second (indicating its row).

**Example 1**

```
Input: coordinate1 = "a1", coordinate2 = "c3"
Output: true
Explanation:
Both squares are black.
```

**Example 2**

```
Input: coordinate1 = "a1", coordinate2 = "h3"
Output: false
Explanation:
Square "a1" is black and "h3" is white.
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2024/12/03
 */
public class CheckSameColor {
    public static boolean checkTwoChessboards(String coordinate1, String coordinate2) {
        // Convert the first character (column) to numeric index
        // 'a' becomes 1, 'b' becomes 2, etc.
        int col1 = coordinate1.charAt(0) - 'a' + 1;

        // Convert the second character (row) to numeric index
        // '1' becomes 1, '2' becomes 2, etc.
        int row1 = coordinate1.charAt(1) - '0';

        // Repeat the same for the second coordinate
        int col2 = coordinate2.charAt(0) - 'a' + 1;
        int row2 = coordinate2.charAt(1) - '0';

        // Core color determination logic:
        // 1. Sum the row and column indexes of both squares
        // 2. Check if the total sum is even or odd
        // 3. Even sum means both squares are the same color (black)
        // 4. Odd sum means the squares have different colors
        //
        // Mathematical explanation:
        // - In a chessboard, square color depends on row + column sum
        // - If row + column is even, the square is black
        // - If row + column is odd, the square is white
        return (row1 + col1 + row2 + col2) % 2 == 0;
    }

    public static void main(String[] args) {
        // Test case 1: Same color squares (both black)
        String coordinate1 = "a1";
        String coordinate2 = "c3";
        boolean result1 = checkTwoChessboards(coordinate1, coordinate2);
        System.out.println(coordinate1 + " and " + coordinate2 + " have the same color: " + result1);

        // Test case 2: Different color squares
        String coordinate3 = "a1";
        String coordinate4 = "h3";
        boolean result2 = checkTwoChessboards(coordinate3, coordinate4);
        System.out.println(coordinate3 + " and " + coordinate4 + " have the same color: " + result2);

        // Test case 3: Border test - first row and last column
        String coordinate5 = "a1";
        String coordinate6 = "h1";
        boolean result3 = checkTwoChessboards(coordinate5, coordinate6);
        System.out.println(coordinate5 + " and " + coordinate6 + " have the same color: " + result3);

        // Test case 4: Middle area test
        String coordinate7 = "d4";
        String coordinate8 = "e5";
        boolean result4 = checkTwoChessboards(coordinate7, coordinate8);
        System.out.println(coordinate7 + " and " + coordinate8 + " have the same color: " + result4);
    }
}

```





