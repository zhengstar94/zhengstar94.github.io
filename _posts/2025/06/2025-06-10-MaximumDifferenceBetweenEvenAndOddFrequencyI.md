---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3442. Maximum Difference Between Even and Odd Frequency I"
date: "2025-06-10"
tags: Medium
categories:
  - "LeetCode Array"
---


- You are given a string `s` consisting of lowercase English letters.
- Your task is to find the **maximum** difference `diff = a1 - a2` between the frequency of characters `a1` and `a2` in the string such that:
  - `a1` has an **odd frequency** in the string.
  - `a2` has an **even frequency** in the string.
- Return this **maximum** difference.

**Example 1**

```
Input: s = "aaaaabbc"

Output: 3

Explanation:

The character 'a' has an odd frequency of 5, and 'b' has an even frequency of 2.
The maximum difference is 5 - 2 = 3.
```

**Example 2**

```
Input: s = "abcabcab"

Output: 1

Explanation:

The character 'a' has an odd frequency of 3, and 'c' has an even frequency of 2.
The maximum difference is 3 - 2 = 1.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @Author zhengxingxing
 * @Date 2025/06/10
 */
public class MaximumDifferenceBetweenEvenAndOddFrequencyI {

    public static int maxDifference(String s) {
        // Step 1: Initialize frequency counter array for 26 lowercase letters
        // Index 0 represents 'a', index 1 represents 'b', ..., index 25 represents 'z'
        // Using array instead of HashMap for better performance and space efficiency
        int[] cnt = new int[26];

        // Step 2: Count frequency of each character in the string
        // Convert string to character array for efficient iteration
        for (char c : s.toCharArray()) {
            // Map character to array index: 'a'->0, 'b'->1, ..., 'z'->25
            // Increment the frequency counter for this character
            cnt[c - 'a']++;
        }

        // Step 3: Initialize variables to track maximum odd frequency and minimum even frequency
        int max1 = 0;  // Maximum odd frequency found so far (initialize to 0 since frequencies are positive)
        int min0 = Integer.MAX_VALUE;  // Minimum even frequency found so far (initialize to max value)

        // Step 4: Iterate through all frequency counts to find max odd and min even
        for (int c : cnt) {
            // Check if current frequency is odd (remainder when divided by 2 is greater than 0)
            if (c % 2 > 0) {
                // Update maximum odd frequency if current frequency is larger
                max1 = Math.max(max1, c);
            }
            // Check if current frequency is even AND greater than 0 (exists in string)
            // We need c > 0 condition to exclude characters that don't appear in the string
            else if (c > 0) {
                // Update minimum even frequency if current frequency is smaller
                min0 = Math.min(min0, c);
            }
        }

        // Step 5: Return the maximum difference
        // This represents the largest possible difference between an odd frequency character
        // and an even frequency character in the string
        return max1 - min0;
    }

    public static void main(String[] args) {
        // Test Case 1: "aaaaabbc"
        // Character frequencies: a=5(odd), b=2(even), c=1(odd)
        // Maximum odd frequency: 5, Minimum even frequency: 2
        // Expected result: 5 - 2 = 3
        String s1 = "aaaaabbc";
        int result1 = maxDifference(s1);
        System.out.println("Test Case 1:");
        System.out.println("Input: " + s1);
        System.out.println("Output: " + result1);
        System.out.println("Expected: 3");
        System.out.println("Result: " + (result1 == 3 ? "PASS" : "FAIL"));
        System.out.println();

        // Test Case 2: "abcabcab"
        // Character frequencies: a=3(odd), b=3(odd), c=2(even)
        // Maximum odd frequency: 3, Minimum even frequency: 2
        // Expected result: 3 - 2 = 1
        String s2 = "abcabcab";
        int result2 = maxDifference(s2);
        System.out.println("Test Case 2:");
        System.out.println("Input: " + s2);
        System.out.println("Output: " + result2);
        System.out.println("Expected: 1");
        System.out.println("Result: " + (result2 == 1 ? "PASS" : "FAIL"));
        System.out.println();
    }
}
```





