---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1759. Count Number of Homogenous Substrings"
date: "2025-04-16"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---


- Given a string `s`, return *the number of **homogenous** substrings of* `s`*.* Since the answer may be too large, return it **modulo** `109 + 7`.
- A string is **homogenous** if all the characters of the string are the same.
- A **substring** is a contiguous sequence of characters within a string.

**Example 1**

```
Input: s = "abbcccaa"
Output: 13
Explanation: The homogenous substrings are listed as below:
"a"   appears 3 times.
"aa"  appears 1 time.
"b"   appears 2 times.
"bb"  appears 1 time.
"c"   appears 3 times.
"cc"  appears 2 times.
"ccc" appears 1 time.
3 + 1 + 2 + 1 + 3 + 2 + 1 = 13.
```

**Example 2**

```
Input: s = "xy"
Output: 2
Explanation: The homogenous substrings are "x" and "y".
```

**Example 3**

```
Input: s = "zzzzz"
Output: 15
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/16
 */
public class CountNumberOfHomogenousSubstrings {
    
    public static int countHomogenous(String s) {
        // Define modulo constant to handle large numbers
        final int MOD = 1_000_000_007;

        // Result variable to store the total count
        long result = 0;
        // Length of input string
        int n = s.length();
        // Current position in string
        int i = 0;

        // Outer loop: process each group of same characters
        while (i < n) {
            // Mark the start of current group
            int start = i;
            // Get the character for current group
            char currChar = s.charAt(start);

            // Inner loop: find the end of current group of same characters
            // Continue while we haven't reached end and characters are same
            while (i < n && s.charAt(i) == currChar) {
                i++;
            }

            // Calculate length of current group of same characters
            long groupLength = i - start;

            // Calculate contribution of current group using formula: n*(n+1)/2
            // This formula gives sum of numbers from 1 to n
            // Apply modulo to handle large numbers
            result = (result + (groupLength * (groupLength + 1) / 2)) % MOD;
        }

        // Convert long to int for final result
        return (int)result;
    }
    
    public static void main(String[] args) {
        // Test case 1: String with multiple groups
        System.out.println("Test Case 1 Result: " + countHomogenous("abbcccaa")); // Expected: 13

        // Test case 2: String with no consecutive same characters
        System.out.println("Test Case 2 Result: " + countHomogenous("xy")); // Expected: 2

        // Test case 3: String with all same characters
        System.out.println("Test Case 3 Result: " + countHomogenous("zzzzz")); // Expected: 15
    }
}

```





