---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2513. Minimize the Maximum of Two Arrays"
date: "2025-05-17"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.MinimizeMaximum"
---


- We have two arrays `arr1` and `arr2` which are initially empty. You need to add positive integers to them such that they satisfy all the following conditions:
  - `arr1` contains `uniqueCnt1` **distinct** positive integers, each of which is **not divisible** by `divisor1`.
  - `arr2` contains `uniqueCnt2` **distinct** positive integers, each of which is **not divisible** by `divisor2`.
  - **No** integer is present in both `arr1` and `arr2`.
- Given `divisor1`, `divisor2`, `uniqueCnt1`, and `uniqueCnt2`, return *the **minimum possible maximum** integer that can be present in either array*.

**Example 1**

```
Input: divisor1 = 2, divisor2 = 7, uniqueCnt1 = 1, uniqueCnt2 = 3
Output: 4
Explanation: 
We can distribute the first 4 natural numbers into arr1 and arr2.
arr1 = [1] and arr2 = [2,3,4].
We can see that both arrays satisfy all the conditions.
Since the maximum value is 4, we return it.
```

**Example 2**

```
Input: divisor1 = 3, divisor2 = 5, uniqueCnt1 = 2, uniqueCnt2 = 1
Output: 3
Explanation: 
Here arr1 = [1,2], and arr2 = [3] satisfy all conditions.
Since the maximum value is 3, we return it.
```

**Example 3**

```
Input: divisor1 = 2, divisor2 = 4, uniqueCnt1 = 8, uniqueCnt2 = 2
Output: 15
Explanation: 
Here, the final possible arrays can be arr1 = [1,3,5,7,9,11,13,15], and arr2 = [2,6].
It can be shown that it is not possible to obtain a lower maximum satisfying all conditions. 
```

## Method 1

```tex
【O(log(divisor1+divisor2)+log(uniqueCnt1+uniqueCnt2)) time | O(1) space】
```

```java
package Leetcode.BinarySearch.MinimizeMaximum;

/**
 * @author zhengxingxing
 * @date 2025/05/17
 */
public class MinimizeTheMaximumOfTwoArrays {

    public static int minimizeSet(int divisor1, int divisor2, int uniqueCnt1, int uniqueCnt2) {
        // Calculate the Least Common Multiple (LCM) of divisor1 and divisor2,
        // used for applying the inclusion-exclusion principle later.
        long lcm = lcm(divisor1, divisor2);

        // Initialize binary search boundaries:
        // left starts at 1 (smallest positive integer),
        // right is a safe upper bound (twice the total count of unique elements needed).
        int left = 1;
        int right = 2 * (uniqueCnt1 + uniqueCnt2);

        // Perform binary search to find the minimal maximum integer x
        // such that it's possible to form arr1 and arr2 under constraints.
        while (left < right) {
            int mid = left + (right - left) / 2;
            // Use the check function to verify if mid can satisfy the constraints.
            if (check(mid, divisor1, divisor2, uniqueCnt1, uniqueCnt2, lcm)) {
                // If possible, try smaller values to minimize the maximum integer.
                right = mid;
            } else {
                // Otherwise, increase the lower bound to look for a larger feasible value.
                left = mid + 1;
            }
        }

        // At the end of binary search, left == right and represents the minimal maximum integer.
        return left;
    }

    private static boolean check(int x, int d1, int d2, int uniqueCnt1, int uniqueCnt2, long lcm) {
        /*
         * Calculate how many integers arr1 still needs to pick after excluding
         * numbers that arr2 will take (numbers divisible by d2, except those divisible by lcm).
         * uniqueCnt1 - (x / d2) + (x / lcm) calculates the "remaining needed" count for arr1.
         *
         * Math.max(..., 0) ensures we don't get negative values when arr2 doesn't take enough to affect arr1.
         */
        long left1 = Math.max(uniqueCnt1 - (x / d2) + (x / lcm), 0);

        /*
         * Similarly, calculate how many integers arr2 still needs to pick after excluding
         * numbers that arr1 will take (numbers divisible by d1, except those divisible by lcm).
         */
        long left2 = Math.max(uniqueCnt2 - (x / d1) + (x / lcm), 0);

        /*
         * Calculate how many numbers in [1, x] are not divisible by either d1 or d2.
         * This uses the inclusion-exclusion principle:
         * - total numbers in [1,x]: x
         * - subtract numbers divisible by d1: x/d1
         * - subtract numbers divisible by d2: x/d2
         * - add back numbers divisible by both (lcm): x/lcm
         *
         * This 'common' count represents the pool of numbers that neither arr1 nor arr2
         * is restricted from using, and hence can be distributed between them.
         */
        long common = x - (x / d1) - (x / d2) + (x / lcm);

        /*
         * Return true if the common pool has enough numbers to satisfy the leftover
         * needs of both arrays without conflicts.
         */
        return common >= left1 + left2;
    }

    /**
     * Calculate the Greatest Common Divisor (GCD) of two numbers using the Euclidean algorithm.
     *
     * @param a First number.
     * @param b Second number.
     * @return The GCD of a and b.
     */
    private static long gcd(long a, long b) {
        // If b is zero, gcd is a; otherwise, recurse with (b, a % b).
        return b == 0 ? a : gcd(b, a % b);
    }

    /**
     * Calculate the Least Common Multiple (LCM) of two numbers.
     * Formula: lcm(a, b) = (a * b) / gcd(a, b)
     *
     * @param a First number.
     * @param b Second number.
     * @return The LCM of a and b.
     */
    private static long lcm(long a, long b) {
        return a * b / gcd(a, b);
    }


    public static void main(String[] args) {
        // Test case 1
        int divisor1 = 2, divisor2 = 7, uniqueCnt1 = 1, uniqueCnt2 = 3;
        System.out.println("Test case 1:");
        System.out.println("Input: divisor1 = " + divisor1 + ", divisor2 = " + divisor2 +
                ", uniqueCnt1 = " + uniqueCnt1 + ", uniqueCnt2 = " + uniqueCnt2);
        System.out.println("Output: " + minimizeSet(divisor1, divisor2, uniqueCnt1, uniqueCnt2));
        System.out.println("Expected: 4");
        System.out.println();

        // Test case 2
        divisor1 = 3;
        divisor2 = 5;
        uniqueCnt1 = 2;
        uniqueCnt2 = 1;
        System.out.println("Test case 2:");
        System.out.println("Input: divisor1 = " + divisor1 + ", divisor2 = " + divisor2 +
                ", uniqueCnt1 = " + uniqueCnt1 + ", uniqueCnt2 = " + uniqueCnt2);
        System.out.println("Output: " + minimizeSet(divisor1, divisor2, uniqueCnt1, uniqueCnt2));
        System.out.println("Expected: 3");
        System.out.println();

        // Test case 3
        divisor1 = 2;
        divisor2 = 4;
        uniqueCnt1 = 8;
        uniqueCnt2 = 2;
        System.out.println("Test case 3:");
        System.out.println("Input: divisor1 = " + divisor1 + ", divisor2 = " + divisor2 +
                ", uniqueCnt1 = " + uniqueCnt1 + ", uniqueCnt2 = " + uniqueCnt2);
        System.out.println("Output: " + minimizeSet(divisor1, divisor2, uniqueCnt1, uniqueCnt2));
        System.out.println("Expected: 15");
    }
}

```





