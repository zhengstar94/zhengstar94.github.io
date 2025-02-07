---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1358. Number of Substrings Containing All Three Characters"
date: "2025-02-03"
tags: Medium SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowCountLongest"
---


- Given a string `s` consisting only of characters *a*, *b* and *c*.
- Return the number of substrings containing **at least** one occurrence of all these characters *a*, *b* and *c*.

**Example 1**

```
Input: s = "abcabc"
Output: 10
Explanation: The substrings containing at least one occurrence of the characters a, b and c are "abc", "abca", "abcab", "abcabc", "bca", "bcab", "bcabc", "cab", "cabc" and "abc" (again). 
```

**Example 2**

```
Input: s = "aaacb"
Output: 3
Explanation: The substrings containing at least one occurrence of the characters a, b and c are "aaacb", "aacb" and "acb". 
```

**Example 3**

```
Input: s = "abc"
Output: 1
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowCountLongest;

/**
 * @author zhengxingxing
 * @date 2025/02/03
 */
public class NumberOfSubstringsContainingAllThreeCharacters {
    public static int numberOfSubstrings(String s) {
        // Array to keep track of frequency of each character (a, b, c) in current window
        // count[0] for 'a', count[1] for 'b', count[2] for 'c'
        int[] count = new int[3];

        // Left pointer of sliding window, used for window contraction
        int left = 0;

        // Variable to store the final result (total count of valid substrings)
        int result = 0;

        // Iterate through string using right pointer to expand window
        for (int right = 0; right < s.length(); right++) {
            // Increment count for current character
            // Subtract 'a' to convert character to index (0 for 'a', 1 for 'b', 2 for 'c')
            count[s.charAt(right) - 'a']++;

            // While window contains all three characters
            while (count[0] > 0 && count[1] > 0 && count[2] > 0) {
                // For current valid window, add number of possible substrings
                // s.length() - right represents number of possible extensions of current valid window
                // Example: for "abcabc", when right=2 ("abc"), possible substrings are:
                // "abc", "abca", "abcab", "abcabc"
                result += s.length() - right;

                // Contract window from left by decreasing count of leftmost character
                count[s.charAt(left) - 'a']--;

                // Move left pointer to continue checking smaller windows
                left++;
            }
        }

        // Return total count of valid substrings
        return result;
    }

    
    public static void main(String[] args) {
        // Test Case 1: Regular case with repeated pattern
        String s1 = "abcabc";
        System.out.println("Test Case 1: " + numberOfSubstrings(s1)); // Expected output: 10

        // Test Case 2: Uneven distribution of characters
        String s2 = "aaacb";
        System.out.println("Test Case 2: " + numberOfSubstrings(s2)); // Expected output: 3

        // Test Case 3: Minimal case with exactly one occurrence of each
        String s3 = "abc";
        System.out.println("Test Case 3: " + numberOfSubstrings(s3)); // Expected output: 1

        // Test Case 4: Invalid case with missing characters
        String s4 = "aaaa";
        System.out.println("Test Case 4: " + numberOfSubstrings(s4)); // Expected output: 0

        // Test Case 5: Longer string with multiple valid substrings
        String s5 = "abcabcabc";
        System.out.println("Test Case 5: " + numberOfSubstrings(s5)); // Expected output: 28
    }
}

```





