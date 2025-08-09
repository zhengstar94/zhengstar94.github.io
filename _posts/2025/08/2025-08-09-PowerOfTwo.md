---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "231. Power of Two"
date: "2025-08-09"
tags: Easy
categories:
  - "LeetCode Math"
---


- Given an integer `n`, return *`true` if it is a power of two. Otherwise, return `false`*.
- An integer `n` is a power of two, if there exists an integer `x` such that `n == 2^x`.

**Example 1**

```
Input: n = 1
Output: true
Explanation: 2^0 = 1
```

**Example 2**

```
Input: n = 16
Output: true
Explanation: 2^4 = 16
```

**Example 2**

```
Input: n = 3
Output: false
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @Author zhengxingxing
 * @Date 2025/08/09
 */
public class PowerOfTwo {

    public static boolean isPowerOfTwo(int n) {
        // First, check if n is greater than 0.
        // Then, use bitwise AND to check if n has only one '1' in its binary representation.
        // For any power of two, n & (n - 1) will be 0.
        // Example: n = 8 (1000), n - 1 = 7 (0111), 1000 & 0111 = 0000
        return n > 0 && (n & (n - 1)) == 0;
    }

    public static void main(String[] args) {
        // Test cases to verify the function
        System.out.println(isPowerOfTwo(1));   // true, because 1 = 2^0
        System.out.println(isPowerOfTwo(16));  // true, because 16 = 2^4
        System.out.println(isPowerOfTwo(3));   // false, because 3 is not a power of two
    }
}
```





