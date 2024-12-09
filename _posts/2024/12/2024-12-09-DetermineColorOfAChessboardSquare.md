---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1812. Determine Color of a Chessboard Square"
date: "2024-12-09"
tags: Easy
categories:
  - "LeetCode Math"
---

- You are given `coordinates`, a string that represents the coordinates of a square of the chessboard. Below is a chessboard for your reference.

- Return `true` *if the square is white, and* `false` *if the square is black*.

  The coordinate will always represent a valid chessboard square. The coordinate will always have the letter first, and the number second.

**Example 1**

```
Input: coordinates = "a1"
Output: false
Explanation: From the chessboard above, the square with coordinates "a1" is black, so return false.
```

**Example 2**

```
Input: coordinates = "h3"
Output: true
Explanation: From the chessboard above, the square with coordinates "h3" is white, so return true.
```

**Example 3**

```
Input: coordinates = "c7"
Output: false
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2024/12/09
 */
public class DetermineColorOfAChessboardSquare {
    public static boolean squareIsWhite(String coordinates) {
        // Convert column letter to zero-based index (a=0, b=1, ...)
        int col = coordinates.charAt(0) - 'a';
        // Convert row number to zero-based index (1=0, 2=1, ...)
        int row = coordinates.charAt(1) - '1';

        // If the sum of column and row indices is odd, the square is white
        return (col + row) % 2 == 1;
    }

    public static void main(String[] args) {
        // Test case 1: Black square
        String test1 = "a1";
        System.out.println("Is coordinate " + test1 + " white? " + squareIsWhite(test1));

        // Test case 2: White square
        String test2 = "h3";
        System.out.println("Is coordinate " + test2 + " white? " + squareIsWhite(test2));

        // Test case 3: Black square
        String test3 = "c7";
        System.out.println("Is coordinate " + test3 + " white? " + squareIsWhite(test3));

        // Additional test case
        String test4 = "e4";
        System.out.println("Is coordinate " + test4 + " white? " + squareIsWhite(test4));
    }
}
```





