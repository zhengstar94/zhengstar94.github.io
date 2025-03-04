---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1780. Check if Number is a Sum of Powers of Three"
date: "2025-03-04"
tags: Medium
categories:
  - "LeetCode Math" 
---


- Given an integer `n`, return `true` *if it is possible to represent* `n` *as the sum of distinct powers of three.* Otherwise, return `false`.
- An integer `y` is a power of three if there exists an integer `x` such that `y == 3x`.

**Example 1**

```
Input: n = 12
Output: true
Explanation: 12 = 3^1 + 3^2
```

**Example 2**

```
Input: n = 91
Output: true
Explanation: 91 = 3^0 + 3^2 + 3^4
```

**Example 3**

```
Input: n = 21
Output: false
```

## Method 1

```tex
【O(log₃n) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/03/04
 */
public class CheckIfNumberIsASumOfPowersOfThree {
    
    public static boolean checkPowersOfThree(int n) {
        while (n > 0) {
            // If any digit in base 3 representation is 2, return false
            if (n % 3 == 2) {
                return false;
            }
            // Continue dividing by 3 to check next digit
            n /= 3;
        }
        return true;
    }
    
    public static void main(String[] args) {
        // Test case 1: n = 12, should return true (12 = 3¹ + 3²)
        System.out.println("Test case 1 (n = 12): " + checkPowersOfThree(12)); // Expected: true

        // Test case 2: n = 91, should return true (91 = 3⁰ + 3² + 3⁴)
        System.out.println("Test case 2 (n = 91): " + checkPowersOfThree(91)); // Expected: true

        // Test case 3: n = 21, should return false (contains 2 in base 3)
        System.out.println("Test case 3 (n = 21): " + checkPowersOfThree(21)); // Expected: false

        // Additional test cases for better coverage
        System.out.println("Test case 4 (n = 45): " + checkPowersOfThree(45)); // Expected: false
        System.out.println("Test case 5 (n = 9): " + checkPowersOfThree(9));   // Expected: true
    }
}

```







