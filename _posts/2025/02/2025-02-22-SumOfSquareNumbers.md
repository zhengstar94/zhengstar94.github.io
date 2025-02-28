---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "633. Sum of Square Numbers"
date: "2025-02-22"
tags: Medium TwoPointers
categories:
  - "LeetCode SingleSeqTwoPointersInward"
---


- Given a non-negative integer `c`, decide whether there're two integers `a` and `b` such that `a^2 + b^2 = c`.


**Example 1**

```
Input: c = 5
Output: true
Explanation: 1 * 1 + 2 * 2 = 5
```

**Example 2**

```
Input: c = 3
Output: false
```

## Method 1

```tex
【O(√c) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2025/02/22
 */
public class SumOfSquareNumbers {

    public static boolean judgeSquareSum(int c) {
        long left = 0;  // Left pointer starts from 0
        long right = (long) Math.sqrt(c);  // Right pointer starts from square root of c

        while (left <= right) {
            long sum = left * left + right * right;
            if (sum == c) {
                return true;    // Found two numbers whose squares sum up to c
            } else if (sum < c) {
                left++;         // Sum is too small, increase left pointer
            } else {
                right--;        // Sum is too large, decrease right pointer
            }
        }
        return false;          // No solution found
    }

    public static void main(String[] args) {
        // Test case 1: c = 5
        int test1 = 5;
        System.out.println("Test case 1 (c = 5): " + judgeSquareSum(test1)); // Expected output: true

        // Test case 2: c = 3
        int test2 = 3;
        System.out.println("Test case 2 (c = 3): " + judgeSquareSum(test2)); // Expected output: false

        // Test case 3: c = 0
        int test3 = 0;
        System.out.println("Test case 3 (c = 0): " + judgeSquareSum(test3)); // Expected output: true

        // Test case 4: c = 1
        int test4 = 1;
        System.out.println("Test case 4 (c = 1): " + judgeSquareSum(test4)); // Expected output: true

        // Test case 5: Large number test
        int test5 = 2147483647;
        System.out.println("Test case 5 (c = 2147483647): " + judgeSquareSum(test5));
    }
}

```





