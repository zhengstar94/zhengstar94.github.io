---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1016. Binary String With Substrings Representing 1 To N"
date: "2025-01-08"
tags: Medium
categories:
  - "LeetCode String"
---


- Given a binary string `s` and a positive integer `n`, return `true` *if the binary representation of all the integers in the range* `[1, n]` *are **substrings** of* `s`*, or* `false` *otherwise*.
- A **substring** is a contiguous sequence of characters within a string.

**Example 1**

```
Input: s = "0110", n = 3
Output: true
```

**Example 2**

```
Input: s = "0110", n = 4
Output: false
```

## Method 1

```tex
【O(n * L) time | O(log(n)) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2025/01/08
 */
public class BinaryStringWithSubstringsRepresenting1ToN {
    public static boolean queryString(String s, int n) {
        // Check if string s contains binary representation of all numbers from 0 to n
        for (int i = 0; i <= n; i++) {
            String binary = Integer.toBinaryString(i);
            if(!s.contains(binary)){
                return false;
            }
        }

        return true;
    }

    public static void main(String[] args) {
        // Test case 1
        String s1 = "0110";
        int n1 = 3;
        System.out.println("Test case 1:");
        System.out.println("Input: s = \"" + s1 + "\", n = " + n1);
        System.out.println("Output: " + queryString(s1, n1));  // Should output true

        // Test case 2
        String s2 = "0110";
        int n2 = 4;
        System.out.println("\nTest case 2:");
        System.out.println("Input: s = \"" + s2 + "\", n = " + n2);
        System.out.println("Output: " + queryString(s2, n2));  // Should output false

        // Additional test case
        String s3 = "1111000";
        int n3 = 5;
        System.out.println("\nTest case 3:");
        System.out.println("Input: s = \"" + s3 + "\", n = " + n3);
        System.out.println("Output: " + queryString(s3, n3));
    }
}
```





