---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "371.Sum of Two Integers"
date: "2024-05-05"
categories:
  - "LeetCode BitManipulation"
---

# 371. Sum of Two Integers

- Given two integers `a` and `b`, return *the sum of the two integers without using the operators* `+` *and* `-`.

**Example 1**

```
Input: a = 1, b = 2
Output: 3
```

**Example 2**

```
Input: a = 2, b = 3
Output: 5
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.BitManipulation;

/**
 * @author zhengstars
 * @date 2024/06/12
 */
public class SumOfTwoIntegers {
    public static int getSum(int a, int b) {
        // Iterate until there is no carry left
        while (b != 0) {
            // Calculate the carry
            // (common set bits of a and b)
            int carry = a & b;

            // Compute the sum without considering the carry
            // XOR operation gives the sum of bits where at least one of the bits is not set
            a = a ^ b;

            // Shift the carry left by one
            // The carry needs to be added to the sum in the next iteration
            b = carry << 1;
        }

        // When b is 0, there is no carry left and a contains the final sum
        return a;
    }

    public static void main(String[] args) {
        // Test case 1: 1 + 2 = 3
        System.out.println("Sum of 1 and 2: " + getSum(1, 2)); // Should output 3

        // Test case 2: 2 + 3 = 5
        System.out.println("Sum of 2 and 3: " + getSum(2, 3)); // Should output 5

        // Test case 3: 11 + 13 = 24
        System.out.println("Sum of 11 and 13: " + getSum(11, 13)); // Should output 24
    }
}

```

