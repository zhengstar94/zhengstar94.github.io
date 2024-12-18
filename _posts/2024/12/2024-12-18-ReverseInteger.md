---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "7. Reverse Integer"
date: "2024-12-18"
tags: Medium
categories:
  - "LeetCode MathGeometry"
---


- Given a signed 32-bit integer `x`, return `x` *with its digits reversed*. If reversing `x` causes the value to go outside the signed 32-bit integer range `[-231, 231 - 1]`, then return `0`.
- **Assume the environment does not allow you to store 64-bit integers (signed or unsigned).**


**Example 1**

```
Input: x = 123
Output: 321
```

**Example 2**

```
Input: x = -123
Output: -321
```

**Example 3**

```
Input: x = 120
Output: 21
```

## Method 1

```tex
【O(log(n)) time | O(1) space】
```

```java
package Leetcode.MathGeometry;

/**
 * @author zhengxingxing
 * @date 2024/12/18
 */
public class ReverseInteger {
    public static int reverse(int x) {
        // Initialize a variable to store the reversed number
        // This will be built digit by digit during the reversal process
        int reversed = 0;

        // Continue the loop until all digits of the input number are processed
        // The loop will terminate when x becomes 0
        while (x != 0) {
            // Extract the last digit of the current number
            // Using modulo 10 operation to get the rightmost digit
            // For positive numbers like 123, this will give 3
            // For negative numbers like -456, this will give -6
            int digit = x % 10;

            // Overflow check for positive integer limits
            // Two conditions to prevent integer overflow:
            // 1. If reversed is already greater than Integer.MAX_VALUE / 10
            // 2. If reversed is equal to Integer.MAX_VALUE / 10 and the next digit
            //    would make it exceed MAX_VALUE
            // MAX_VALUE is 2147483647, so the last digit can't be greater than 7
            if (reversed > Integer.MAX_VALUE / 10 ||
                    (reversed == Integer.MAX_VALUE / 10 && digit > 7)) {
                // If overflow would occur, return 0 as per the problem specification
                return 0;
            }

            // Overflow check for negative integer limits
            // Similar to positive overflow check, but for negative numbers
            // MIN_VALUE is -2147483648, so the last digit can't be less than -8
            if (reversed < Integer.MIN_VALUE / 10 ||
                    (reversed == Integer.MIN_VALUE / 10 && digit < -8)) {
                // If overflow would occur, return 0
                return 0;
            }

            // Build the reversed number
            // This is done by:
            // 1. Multiplying the current reversed number by 10 (shifting left)
            // 2. Adding the new digit
            // Example progression:
            // First iteration: 0 * 10 + 3 = 3
            // Second iteration: 3 * 10 + 2 = 32
            // Third iteration: 32 * 10 + 1 = 321
            reversed = reversed * 10 + digit;

            // Remove the last digit from the original number
            // Integer division by 10 effectively drops the last digit
            // 123 / 10 = 12
            // -456 / 10 = -45
            x /= 10;
        }

        // Return the fully reversed number
        return reversed;
    }
    
    public static void main(String[] args) {

        // Test case 1: Normal positive number
        int num1 = 123;
        System.out.println(num1 + " reversed: " + reverse(num1));

        // Test case 2: Negative number
        int num2 = -456;
        System.out.println(num2 + " reversed: " + reverse(num2));

        // Test case 3: Number with trailing zeros
        int num3 = 1200;
        System.out.println(num3 + " reversed: " + reverse(num3));

        // Test case 4: Number that would cause overflow
        int num4 = 1534236469;
        System.out.println(num4 + " reversed: " + reverse(num4));

        // Test case 5: Zero
        int num5 = 0;
        System.out.println(num5 + " reversed: " + reverse(num5));

        // Test case 6: Negative number that would cause overflow
        int num6 = -2147483648;
        System.out.println(num6 + " reversed: " + reverse(num6));
    }
}

```





