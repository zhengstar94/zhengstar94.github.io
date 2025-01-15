---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2429. Minimize XOR"
date: "2025-01-15"
tags: Medium
categories:
  - "LeetCode Math"
---


- Given two positive integers `num1` and `num2`, find the positive integer `x` such that:
  - `x` has the same number of set bits as `num2`, and
  - The value `x XOR num1` is **minimal**.
- Note that `XOR` is the bitwise XOR operation.
- Return *the integer* `x`. The test cases are generated such that `x` is **uniquely determined**.
- The number of **set bits** of an integer is the number of `1`'s in its binary representation.

**Example 1**

```
Input: num1 = 3, num2 = 5
Output: 3
Explanation:
The binary representations of num1 and num2 are 0011 and 0101, respectively.
The integer 3 has the same number of set bits as num2, and the value 3 XOR 3 = 0 is minimal.
```

**Example 2**

```
Input: num1 = 1, num2 = 12
Output: 3
Explanation:
The binary representations of num1 and num2 are 0001 and 1100, respectively.
The integer 3 has the same number of set bits as num2, and the value 3 XOR 1 = 2 is minimal.
```

## Method 1

```tex
【O(log(n)) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/01/15
 */
public class MinimizeXOR {

    public static int minimizeXor(int num1, int num2) {
        // Count the number of set bits (1's) in both numbers using built-in function
        int cnt1 = Integer.bitCount(num1); // Count of 1's in num1's binary representation
        int cnt2 = Integer.bitCount(num2); // Count of 1's in num2's binary representation

        // If num1 has more 1's than needed, remove excess 1's from lowest positions
        while (cnt1 > cnt2) {
            // Bitwise operation: num1 & (num1 - 1)
            // Example: if num1 = 1100 (12), then num1 - 1 = 1011
            // 1100 & 1011 = 1000 (removes the lowest 1)
            num1 &= num1 - 1;  // Clear the lowest set bit (1) in num1
            cnt1--;            // Decrement the count of 1's
        }

        // If num1 has fewer 1's than needed, add 1's starting from lowest positions
        while (cnt1 < cnt2) {
            // Bitwise operation: num1 | (num1 + 1)
            // Example: if num1 = 1000, then num1 + 1 = 1001
            // 1000 | 1001 = 1001 (sets the lowest 0 to 1)
            num1 |= num1 + 1;  // Set the lowest unset bit (0) to 1
            cnt1++;            // Increment the count of 1's
        }

        // Return the adjusted num1 which now has the same number of 1's as num2
        // and minimizes XOR with the original num1
        return num1;
    }

    public static void main(String[] args) {
        // Test Case 1
        int num1_1 = 3;
        int num2_1 = 5;
        System.out.println("Test Case 1 Result: " + minimizeXor(num1_1, num2_1)); // Expected Output: 3

        // Test Case 2
        int num1_2 = 1;
        int num2_2 = 12;
        System.out.println("Test Case 2 Result: " + minimizeXor(num1_2, num2_2)); // Expected Output: 3
    }
}
```





