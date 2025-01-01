---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1422. Maximum Score After Splitting a String"
date: "2025-01-01"
tags: Easy
categories:
  - "LeetCode Dynamic Programming"
---


- Given a string `s` of zeros and ones, *return the maximum score after splitting the string into two **non-empty** substrings* (i.e. **left** substring and **right** substring).
- The score after splitting a string is the number of **zeros** in the **left** substring plus the number of **ones** in the **right** substring.

**Example 1**

```
Input: s = "011101"
Output: 5 
Explanation: 
All possible ways of splitting s into two non-empty substrings are:
left = "0" and right = "11101", score = 1 + 4 = 5 
left = "01" and right = "1101", score = 1 + 3 = 4 
left = "011" and right = "101", score = 1 + 2 = 3 
left = "0111" and right = "01", score = 1 + 1 = 2 
left = "01110" and right = "1", score = 2 + 1 = 3
```

**Example 2**

```
Input: s = "00111"
Output: 5
Explanation: When left = "00" and right = "111", we get the maximum score = 2 + 3 = 5
```

**Example 3**

```
Input: s = "1111"
Output: 3
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * @author zhengxingxing
 * @date 2025/01/01
 */
public class MaximumScoreAfterSplittingaString {
    public static int maxScore(String S) {
        char[] s = S.toCharArray();

        // Count initial number of ones (initial right substring score)
        int score = 0;
        for (char c: s){
            score += c - '0';
        }

        // Find maximum score by trying each split position
        int ans = 0;
        for (int i = 0; i < s.length - 1; i++) {
            score += s[i] == '0' ? 1 : -1;  // Adjust score based on current character
            ans = Math.max(ans, score);      // Update maximum score if current is higher
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test case 1: Mixed zeros and ones
        String s1 = "011101";
        System.out.println("Test case 1: " + s1);
        System.out.println("Expected: 5");
        System.out.println("Result: " + maxScore(s1));

        // Test case 2: Zeros followed by ones
        String s2 = "00111";
        System.out.println("\nTest case 2: " + s2);
        System.out.println("Expected: 5");
        System.out.println("Result: " + maxScore(s2));

        // Test case 3: All ones
        String s3 = "1111";
        System.out.println("\nTest case 3: " + s3);
        System.out.println("Expected: 3");
        System.out.println("Result: " + maxScore(s3));
    }
}

```





