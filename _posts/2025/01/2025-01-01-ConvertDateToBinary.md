---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3280. Convert Date to Binary"
date: "2025-01-01"
tags: Easy
categories:
  - "LeetCode Math"
---

- You are given a string `date` representing a Gregorian calendar date in the `yyyy-mm-dd` format.
- `date` can be written in its binary representation obtained by converting year, month, and day to their binary representations without any leading zeroes and writing them down in `year-month-day` format.
- Return the **binary** representation of `date`.

**Example 1**

```
Input: date = "2080-02-29"

Output: "100000100000-10-11101"

Explanation:

100000100000, 10, and 11101 are the binary representations of 2080, 02, and 29 respectively.
```

**Example 2**

```
Input: date = "1900-01-01"

Output: "11101101100-1-1"

Explanation:

11101101100, 1, and 1 are the binary representations of 1900, 1, and 1 respectively.
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/01/01
 */
public class ConvertDateToBinary {
    public static String convertToBinary(String date) {
        // Split the date string
        String[] parts = date.split("-");

        // Convert year, month, and day to integers
        int year = Integer.parseInt(parts[0]);
        int month = Integer.parseInt(parts[1]);
        int day = Integer.parseInt(parts[2]);

        // Convert to binary and remove leading zeros
        String binaryYear = Integer.toBinaryString(year);
        String binaryMonth = Integer.toBinaryString(month);
        String binaryDay = Integer.toBinaryString(day);

        // Concatenate the result in the required format
        return binaryYear + "-" + binaryMonth + "-" + binaryDay;
    }

    public static void main(String[] args) {
        // Test case 1
        String date1 = "2080-02-29";
        System.out.println("Test case 1:");
        System.out.println("Input: " + date1);
        System.out.println("Output: " + convertToBinary(date1));

        // Test case 2
        String date2 = "1900-01-01";
        System.out.println("\nTest case 2:");
        System.out.println("Input: " + date2);
        System.out.println("Output: " + convertToBinary(date2));

        // Additional test case
        String date3 = "2024-03-20";
        System.out.println("\nTest case 3:");
        System.out.println("Input: " + date3);
        System.out.println("Output: " + convertToBinary(date3));
    }
}
```





