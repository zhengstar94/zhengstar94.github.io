---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3325. Count Substrings With K-Frequency Characters I"
date: "2025-02-04"
tags: Medium SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowCountLongest"
---


- Given a string `s` and an integer `k`, return the total number of substrings of `s` where **at least one** character appears **at least** `k` times.

**Example 1**

```
Input: s = "abacb", k = 2

Output: 4

Explanation:

The valid substrings are:

"aba" (character 'a' appears 2 times).
"abac" (character 'a' appears 2 times).
"abacb" (character 'a' appears 2 times).
"bacb" (character 'b' appears 2 times).
```

**Example 2**

```
Input: s = "abcde", k = 1

Output: 15

Explanation:

All substrings are valid because every character appears at least once.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowCountLongest;

/**
 * @author zhengxingxing
 * @date 2025/02/04
 */
public class CountSubstringsWithKFrequencyCharactersI {
    
    public static int numberOfSubstrings(String s, int k) {
        // Array to store frequency of each character (a-z)
        int[] charFreq = new int[26];
        // Left pointer of sliding window
        int left = 0;
        // Right pointer of sliding window
        int right = 0;
        // Store the final result
        int result = 0;

        // Expand window by moving right pointer
        while (right < s.length()) {
            // Increment frequency of current character at right pointer
            // charFreq[0] represents 'a', charFreq[1] represents 'b', and so on
            charFreq[s.charAt(right) - 'a']++;

            // When frequency of current character equals k,
            // contract window from left until its frequency becomes less than k
            // This is because we want to count substrings where character appears exactly k times
            while (charFreq[s.charAt(right) - 'a'] == k) {
                // Decrement frequency of character at left pointer
                // as we're removing it from our window
                charFreq[s.charAt(left) - 'a']--;
                // Move left pointer ahead to shrink window
                left++;
            }

            // Add left pointer value to result
            // This counts all valid substrings ending at right pointer
            result += left;
            // Move right pointer ahead to expand window
            right++;
        }

        return result;
    }

    public static void main(String[] args) {
        // Test case 1
        String s1 = "abacb";
        int k1 = 2;
        System.out.println("Test case 1 result: " + numberOfSubstrings(s1, k1)); // Expected output: 4

        // Test case 2
        String s2 = "abcde";
        int k2 = 1;
        System.out.println("Test case 2 result: " + numberOfSubstrings(s2, k2)); // Expected output: 15

        // Test case 3
        String s3 = "aaa";
        int k3 = 2;
        System.out.println("Test case 3 result: " + numberOfSubstrings(s3, k3)); // Expected output: 3
    }
}

```





