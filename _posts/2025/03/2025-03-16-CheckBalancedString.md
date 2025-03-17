---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3340. Check Balanced String"
date: "2025-03-16"
tags: Easy
categories:
  - "LeetCode Math"
---


- You are given a string `num` consisting of only digits. A string of digits is called **balanced** if the sum of the digits at even indices is equal to the sum of digits at odd indices.
- Return `true` if `num` is **balanced**, otherwise return `false`.

**Example 1**

```
Input: num = "1234"

Output: false

Explanation:

The sum of digits at even indices is 1 + 3 == 4, and the sum of digits at odd indices is 2 + 4 == 6.
Since 4 is not equal to 6, num is not balanced.
```

**Example 2**

```
Input: num = "24123"

Output: true

Explanation:

The sum of digits at even indices is 2 + 1 + 3 == 6, and the sum of digits at odd indices is 4 + 2 == 6.
Since both are equal the num is balanced.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/03/16
 */
public class CheckBalancedString {
    
    public static boolean isBalanced(String num) {
        // Variable to store sum of digits at even indices (0,2,4...)
        int evenSum = 0;
        // Variable to store sum of digits at odd indices (1,3,5...)
        int oddSum = 0;

        // Iterate through each character in the string
        for (int i = 0; i < num.length(); i++) {
            if (i % 2 == 0) {
                // For even indices, add digit to evenSum
                // Subtract '0' to convert char to corresponding integer
                evenSum += num.charAt(i) - '0';
            } else {
                // For odd indices, add digit to oddSum
                oddSum += num.charAt(i) - '0';
            }
        }

        // Return true if sums are equal, false otherwise
        return evenSum == oddSum;
    }

    /**
     * Main method containing test cases
     * Tests the isBalanced method with various inputs
     */
    public static void main(String[] args) {
        // Test Case 1: Regular case where string is not balanced
        String num1 = "1234";
        System.out.println("Test Case 1: " + num1);
        System.out.println("Expected Result: false");
        System.out.println("Actual Result: " + isBalanced(num1));

        // Test Case 2: Regular case where string is balanced
        String num2 = "24123";
        System.out.println("\nTest Case 2: " + num2);
        System.out.println("Expected Result: true");
        System.out.println("Actual Result: " + isBalanced(num2));

        // Test Case 3: Edge case - empty string
        String num3 = "";
        System.out.println("\nTest Case 3: " + num3);
        System.out.println("Expected Result: true");
        System.out.println("Actual Result: " + isBalanced(num3));

        // Test Case 4: Edge case - single digit
        String num4 = "1";
        System.out.println("\nTest Case 4: " + num4);
        System.out.println("Expected Result: true");
        System.out.println("Actual Result: " + isBalanced(num4));
    }
}

```





