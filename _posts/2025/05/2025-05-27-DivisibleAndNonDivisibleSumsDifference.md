---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2894. Divisible and Non-divisible Sums Differencer"
date: "2025-05-27"
tags: Easy
categories:
  - "LeetCode Array"
---


- You are given positive integers `n` and `m`.
- Define two integers as follows:
  - `num1`: The sum of all integers in the range `[1, n]` (both **inclusive**) that are **not divisible** by `m`.
  - `num2`: The sum of all integers in the range `[1, n]` (both **inclusive**) that are **divisible** by `m`.
- Return *the integer* `num1 - num2`.

**Example 1**

```
Input: n = 10, m = 3
Output: 19
Explanation: In the given example:
- Integers in the range [1, 10] that are not divisible by 3 are [1,2,4,5,7,8,10], num1 is the sum of those integers = 37.
- Integers in the range [1, 10] that are divisible by 3 are [3,6,9], num2 is the sum of those integers = 18.
We return 37 - 18 = 19 as the answer.
```

**Example 2**

```
Input: n = 5, m = 6
Output: 15
Explanation: In the given example:
- Integers in the range [1, 5] that are not divisible by 6 are [1,2,3,4,5], num1 is the sum of those integers = 15.
- Integers in the range [1, 5] that are divisible by 6 are [], num2 is the sum of those integers = 0.
We return 15 - 0 = 15 as the answer.
```

**Example 3**

```
Input: n = 5, m = 1
Output: -15
Explanation: In the given example:
- Integers in the range [1, 5] that are not divisible by 1 are [], num1 is the sum of those integers = 0.
- Integers in the range [1, 5] that are divisible by 1 are [1,2,3,4,5], num2 is the sum of those integers = 15.
We return 0 - 15 = -15 as the answer.
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @Author zhengxingxing
 * @Date 2025/05/27
 */
public class DivisibleAndNonDivisibleSumsDifference {

    public static int differenceOfSums(int n, int m) {
        // Calculate the sum of all numbers in range [1, n] using arithmetic series formula: n * (n + 1) / 2
        int totalSum = n * (n + 1) / 2;

        // Calculate how many numbers in range [1, n] are divisible by m
        // This gives us the count of multiples: m, 2m, 3m, ..., count*m where count*m <= n
        int count = n / m;

        // Calculate the sum of all numbers divisible by m
        // Numbers divisible by m: m + 2m + 3m + ... + count*m
        // Factor out m: m * (1 + 2 + 3 + ... + count)
        // Apply arithmetic series formula: m * count * (count + 1) / 2
        int divisibleSum = m * count * (count + 1) / 2;

        // Calculate the sum of numbers NOT divisible by m
        // This equals total sum minus sum of divisible numbers
        int nonDivisibleSum = totalSum - divisibleSum;

        // Return the difference: sum of non-divisible numbers minus sum of divisible numbers
        return nonDivisibleSum - divisibleSum;

        // Alternative simplified calculation: return totalSum - 2 * divisibleSum;
    }

    public static void main(String[] args) {
        // Test case 1: Example from problem description
        int n1 = 10, m1 = 3;
        int result1 = differenceOfSums(n1, m1);
        System.out.println("Test case 1:");
        System.out.println("Input: n = " + n1 + ", m = " + m1);
        System.out.println("Output: " + result1);
        System.out.println("Expected: 19");
        System.out.println("Result: " + (result1 == 19 ? "PASS" : "FAIL"));
        System.out.println();

        // Test case 2: Example from problem description
        int n2 = 5, m2 = 6;
        int result2 = differenceOfSums(n2, m2);
        System.out.println("Test case 2:");
        System.out.println("Input: n = " + n2 + ", m = " + m2);
        System.out.println("Output: " + result2);
        System.out.println("Expected: 15");
        System.out.println("Result: " + (result2 == 15 ? "PASS" : "FAIL"));
        System.out.println();

        // Test case 3: Example from problem description
        int n3 = 5, m3 = 1;
        int result3 = differenceOfSums(n3, m3);
        System.out.println("Test case 3:");
        System.out.println("Input: n = " + n3 + ", m = " + m3);
        System.out.println("Output: " + result3);
        System.out.println("Expected: -15");
        System.out.println("Result: " + (result3 == -15 ? "PASS" : "FAIL"));
        System.out.println();
    }
}
```





