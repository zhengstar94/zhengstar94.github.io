---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1742. Maximum Number of Balls in a Box"
date: "2025-02-13"
tags: Easy
categories:
  - "LeetCode Math"
---


- You are working in a ball factory where you have `n` balls numbered from `lowLimit` up to `highLimit` **inclusive** (i.e., `n == highLimit - lowLimit + 1`), and an infinite number of boxes numbered from `1` to `infinity`.
- Your job at this factory is to put each ball in the box with a number equal to the sum of digits of the ball's number. For example, the ball number `321` will be put in the box number `3 + 2 + 1 = 6` and the ball number `10` will be put in the box number `1 + 0 = 1`.
- Given two integers `lowLimit` and `highLimit`, return *the number of balls in the box with the most balls.*


**Example 1**

```
Input: lowLimit = 1, highLimit = 10
Output: 2
Explanation:
Box Number:  1 2 3 4 5 6 7 8 9 10 11 ...
Ball Count:  2 1 1 1 1 1 1 1 1 0  0  ...
Box 1 has the most number of balls with 2 balls.
```

**Example 2**

```
Input: lowLimit = 5, highLimit = 15
Output: 2
Explanation:
Box Number:  1 2 3 4 5 6 7 8 9 10 11 ...
Ball Count:  1 1 1 1 2 2 1 1 1 0  0  ...
Boxes 5 and 6 have the most number of balls with 2 balls in each.
```

**Example 3**

```
Input: lowLimit = 19, highLimit = 28
Output: 2
Explanation:
Box Number:  1 2 3 4 5 6 7 8 9 10 11 12 ...
Ball Count:  0 1 1 1 1 1 1 1 1 2  0  0  ...
Box 10 has the most number of balls with 2 balls.
```

## Method 1

```tex
【O(n * logm) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/02/13
 */
public class MaximumNumberOfBallsInABox {
    
    public static int countBalls(int lowLimit, int highLimit) {
        // Array to store the count of balls in each box
        // Size is 46 because max sum of digits for 100000 is 9*5=45
        int[] boxes = new int[46];

        // Iterate through each ball number
        for (int i = lowLimit; i <= highLimit; i++) {
            // Increment the count in the box corresponding to the digit sum
            boxes[getDigitSum(i)]++;
        }

        // Find the maximum number of balls in any box
        int maxBalls = 0;
        for (int count : boxes) {
            maxBalls = Math.max(maxBalls, count);
        }

        return maxBalls;
    }
    
    private static int getDigitSum(int num) {
        int sum = 0;
        // Extract each digit and add to sum
        while (num > 0) {
            sum += num % 10;    // Get the last digit
            num /= 10;          // Remove the last digit
        }
        return sum;
    }
    
    public static void main(String[] args) {
        // Test Case 1
        System.out.println("Test Case 1: " + countBalls(1, 10));  // Expected output: 2

        // Test Case 2
        System.out.println("Test Case 2: " + countBalls(5, 15));  // Expected output: 2

        // Test Case 3
        System.out.println("Test Case 3: " + countBalls(19, 28)); // Expected output: 2
    }
}

```





