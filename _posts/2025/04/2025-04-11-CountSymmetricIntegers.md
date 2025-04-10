---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2843. Count Symmetric Integers"
date: "2025-04-11"
tags: Easy
categories:
  - "LeetCode BruteForce"
---

- You are given two positive integers `low` and `high`.
- An integer `x` consisting of `2 * n` digits is **symmetric** if the sum of the first `n` digits of `x` is equal to the sum of the last `n` digits of `x`. Numbers with an odd number of digits are never symmetric.
- Return *the **number of symmetric** integers in the range* `[low, high]`.

**Example 1**

```
Input: low = 1, high = 100
Output: 9
Explanation: There are 9 symmetric integers between 1 and 100: 11, 22, 33, 44, 55, 66, 77, 88, and 99.
```

**Example 2**

```
Input: low = 1200, high = 1230
Output: 4
Explanation: There are 4 symmetric integers between 1200 and 1230: 1203, 1212, 1221, and 1230.
```

## Method 1

```tex
【O((high - low) * d) time | O(1) space】
```

```java
package Leetcode.BruteForce;

/**
 * @author zhengxingxing
 * @date 2025/04/11
 */
public class CountSymmetricIntegers {

    public static int countSymmetricIntegers(int low, int high) {
        int count = 0;  // Counter for symmetric integers

        // Iterate through each number in the range
        for (int num = low; num <= high; num++) {
            String numStr = String.valueOf(num);  // Convert number to string for digit manipulation
            int len = numStr.length();  // Get length of the number

            // Skip numbers with odd number of digits as they can't be symmetric
            if(len % 2 != 0 ){
                continue;
            }

            // Calculate half length for splitting the number
            int halfLen = len / 2;
            int leftSum = 0;   // Sum of digits in first half
            int rightSum = 0;  // Sum of digits in second half

            // Calculate sums of both halves
            for (int i = 0; i < halfLen; i++) {
                leftSum += numStr.charAt(i) - '0';  // Add digit from first half
                rightSum += numStr.charAt(i + halfLen) - '0';  // Add digit from second half
            }

            // If sums are equal, increment counter
            if (leftSum == rightSum){
                count++;
            }
        }

        return count;
    }


    public static void main(String[] args) {
        // Test Case 1: Numbers from 1 to 100
        System.out.println("Test Case 1:");
        int low1 = 1, high1 = 100;
        System.out.println("Input: low = " + low1 + ", high = " + high1);
        System.out.println("Output: " + countSymmetricIntegers(low1, high1));

        // Test Case 2: Numbers from 1200 to 1230
        System.out.println("\nTest Case 2:");
        int low2 = 1200, high2 = 1230;
        System.out.println("Input: low = " + low2 + ", high = " + high2);
        System.out.println("Output: " + countSymmetricIntegers(low2, high2));
    }
}

```





