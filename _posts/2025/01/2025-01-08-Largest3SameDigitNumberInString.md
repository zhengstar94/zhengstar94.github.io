---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2264. Largest 3-Same-Digit Number in String"
date: "2025-01-08"
tags: Easy
categories:
  - "LeetCode Array"
---


- You are given a string `num` representing a large integer. An integer is **good** if it meets the following conditions:
  - It is a **substring** of `num` with length `3`.
  - It consists of only one unique digit.
- Return *the **maximum good** integer as a **string** or an empty string* `""` *if no such integer exists*.
- Note:
  - A **substring** is a contiguous sequence of characters within a string.
  - There may be **leading zeroes** in `num` or a good integer.

**Example 1**

```
Input: num = "6777133339"
Output: "777"
Explanation: There are two distinct good integers: "777" and "333".
"777" is the largest, so we return "777".
```

**Example 2**

```
Input: num = "2300019"
Output: "000"
Explanation: "000" is the only good integer.
```

**Example 3**

```
Input: num = "42352338"
Output: ""
Explanation: No substring of length 3 consists of only one unique digit. Therefore, there are no good integers.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2025/01/08
 */
public class Largest3SameDigitNumberInString {
    
    public static String largestGoodInteger(String num) {
        char maxDigit = '\0';  // Initialize with null character to track the largest digit found

        // Check each possible substring of length 3
        for (int i = 0; i <= num.length() - 3; i++) {
            if (num.charAt(i) == num.charAt(i + 1) &&
                    num.charAt(i) == num.charAt(i + 2)) {
                maxDigit = (char) Math.max(maxDigit, num.charAt(i));
            }
        }

        // Return empty string if no valid substring found
        if (maxDigit == '\0') {
            return "";
        }

        // Build the result string using StringBuilder
        return new StringBuilder()
                .append(maxDigit)
                .append(maxDigit)
                .append(maxDigit)
                .toString();
    }

    public static void main(String[] args) {
        // Test case 1: Multiple valid substrings
        String num1 = "6777133339";
        System.out.println("Test case 1: " + num1);
        System.out.println("Expected output: 777");
        System.out.println("Actual output: " + largestGoodInteger(num1));
        System.out.println();

        // Test case 2: Leading zeros
        String num2 = "2300019";
        System.out.println("Test case 2: " + num2);
        System.out.println("Expected output: 000");
        System.out.println("Actual output: " + largestGoodInteger(num2));
        System.out.println();

        // Test case 3: No valid substring
        String num3 = "42352338";
        System.out.println("Test case 3: " + num3);
        System.out.println("Expected output: ");
        System.out.println("Actual output: " + largestGoodInteger(num3));
    }
}
```





