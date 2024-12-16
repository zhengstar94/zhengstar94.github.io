---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "415. Add Strings"
date: "2024-12-16"
tags: Easy
categories:
  - "LeetCode MathGeometry"
---


- Given two non-negative integers, `num1` and `num2` represented as string, return *the sum of* `num1` *and* `num2` *as a string*.
- You must solve the problem without using any built-in library for handling large integers (such as `BigInteger`). You must also not convert the inputs to integers directly.


**Example 1**

```
Input: num1 = "11", num2 = "123"
Output: "134"
```

**Example 2**

```
Input: num1 = "456", num2 = "77"
Output: "533"
```

**Example 3**

```
Input: num1 = "0", num2 = "0"
Output: "0"
```

## Method 1

```tex
【O(max(n, m)) time | O(max(n, m)) space】
```

```java
package Leetcode.MathGeometry;

/**
 * @author zhengxingxing
 * @date 2024/12/16
 */
public class AddStrings {
    
    public static String addStrings(String num1, String num2) {
        // Initialize pointers to the end of both input strings
        int i = num1.length() - 1, j = num2.length() - 1;

        // Initialize carry variable to track digit overflow
        int up = 0;

        // StringBuilder to efficiently build the result string
        StringBuilder builder = new StringBuilder();

        // Continue while there are digits to process or there's a carry
        while (i >= 0 || j >= 0 || up != 0) {
            // Extract current digits, use 0 if no more digits
            // Convert character to integer by subtracting '0'
            int a = i >= 0 ? num1.charAt(i) - '0' : 0;
            int b = j >= 0 ? num2.charAt(j) - '0' : 0;

            // Calculate sum of current digits and carry
            int result = a + b + up;

            // Append the current digit's ones place to result
            builder.append(result % 10);

            // Calculate new carry for next iteration
            up = result / 10;

            // Move pointers to previous digits
            i--;
            j--;
        }

        // Reverse the string as we built it from least significant digit
        return builder.reverse().toString();
    }
    
    public static void main(String[] args) {

        // Test Case 1: Regular numbers with different lengths
        String test1Num1 = "11";
        String test1Num2 = "123";
        System.out.println("Test 1: " + test1Num1 + " + " + test1Num2 + " = " +
                addStrings(test1Num1, test1Num2));

        // Test Case 2: Numbers with carry
        String test2Num1 = "456";
        String test2Num2 = "77";
        System.out.println("Test 2: " + test2Num1 + " + " + test2Num2 + " = " +
                addStrings(test2Num1, test2Num2));

        // Test Case 3: Zero cases
        String test3Num1 = "0";
        String test3Num2 = "0";
        System.out.println("Test 3: " + test3Num1 + " + " + test3Num2 + " = " +
                addStrings(test3Num1, test3Num2));

        // Test Case 4: Large numbers
        String test4Num1 = "9999";
        String test4Num2 = "1";
        System.out.println("Test 4: " + test4Num1 + " + " + test4Num2 + " = " +
                addStrings(test4Num1, test4Num2));
    }

}

```





