---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2414. Length of the Longest Alphabetical Continuous Substring"
date: "2025-04-09"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---


- An **alphabetical continuous string** is a string consisting of consecutive letters in the alphabet. In other words, it is any substring of the string `"abcdefghijklmnopqrstuvwxyz"`.
  - For example, `"abc"` is an alphabetical continuous string, while `"acb"` and `"za"` are not.
- Given a string `s` consisting of lowercase letters only, return the *length of the **longest** alphabetical continuous substring.*

**Example 1**

```
Input: s = "abacaba"
Output: 2
Explanation: There are 4 distinct continuous substrings: "a", "b", "c" and "ab".
"ab" is the longest continuous substring.
```

**Example 2**

```
Input: s = "abcde"
Output: 5
Explanation: "abcde" is the longest continuous substring.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/09
 */
public class LengthOfTheLongestAlphabeticalContinuousSubstring {
    
    public static int longestContinuousSubstring(String s) {
        // Handle edge cases
        if (s == null || s.length() == 0){
            return 0;
        }

        // Initialize variables
        int n = s.length();          // Length of the string
        int maxLength = 1;           // Track maximum length found
        int i = 0;                   // Current position in string

        // Process string in groups
        while (i < n){
            int start = i;           // Start of current group

            // Extend the current group while characters are consecutive
            // i + 1 < n ensures we don't access beyond string bounds
            // charAt(i + 1) == charAt(i) + 1 checks if next char is consecutive
            while (i + 1 < n && s.charAt(i + 1) == s.charAt(i) + 1){
                i++;
            }

            // Calculate length of current group
            // Add 1 to include the starting character
            int groupLength = i - start + 1;

            // Update maximum length if current group is longer
            maxLength = Math.max(maxLength, groupLength);

            // Move to start of next group
            i++;
        }
        return maxLength;
    }

    public static void main(String[] args) {
        // Test Case 1: Multiple continuous substrings of different lengths
        String test1 = "abacaba";
        System.out.println("Test 1: " + test1);
        System.out.println("Expected: 2");
        System.out.println("Actual: " + longestContinuousSubstring(test1));
        System.out.println();

        // Test Case 2: Perfect continuous string
        String test2 = "abcde";
        System.out.println("Test 2: " + test2);
        System.out.println("Expected: 5");
        System.out.println("Actual: " + longestContinuousSubstring(test2));
        System.out.println();

        // Test Case 3: Single character
        String test3 = "a";
        System.out.println("Test 3: " + test3);
        System.out.println("Expected: 1");
        System.out.println("Actual: " + longestContinuousSubstring(test3));
        System.out.println();

        // Test Case 4: Descending order string
        String test4 = "zyxw";
        System.out.println("Test 4: " + test4);
        System.out.println("Expected: 1");
        System.out.println("Actual: " + longestContinuousSubstring(test4));
        System.out.println();

        // Test Case 5: Multiple continuous segments
        String test5 = "abcdeijkl";
        System.out.println("Test 5: " + test5);
        System.out.println("Expected: 5");
        System.out.println("Actual: " + longestContinuousSubstring(test5));
        System.out.println();
    }
}

```

