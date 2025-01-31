---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "(Review)2904. Shortest and Lexicographically Smallest Beautiful String"
date: "2025-01-31"
tags: Medium Review
categories:
  - "LeetCode DynamicSlidingWindowMin"
---

- You are given a binary string `s` and a positive integer `k`.
- A substring of `s` is **beautiful** if the number of `1`'s in it is exactly `k`.
- Let `len` be the length of the **shortest** beautiful substring.
- Return *the lexicographically **smallest** beautiful substring of string* `s` *with length equal to* `len`. If `s` doesn't contain a beautiful substring, return *an **empty** string*.
- A string `a` is lexicographically **larger** than a string `b` (of the same length) if in the first position where `a` and `b` differ, `a` has a character strictly larger than the corresponding character in `b`.
  - For example, `"abcd"` is lexicographically larger than `"abcc"` because the first position they differ is at the fourth character, and `d` is greater than `c`.


**Example 1**

```
Input: s = "100011001", k = 3
Output: "11001"
Explanation: There are 7 beautiful substrings in this example:
1. The substring "100011001".
2. The substring "100011001".
3. The substring "100011001".
4. The substring "100011001".
5. The substring "100011001".
6. The substring "100011001".
7. The substring "100011001".
The length of the shortest beautiful substring is 5.
The lexicographically smallest beautiful substring with length 5 is the substring "11001".
```

**Example 2**

```
Input: s = "1011", k = 2
Output: "11"
Explanation: There are 3 beautiful substrings in this example:
1. The substring "1011".
2. The substring "1011".
3. The substring "1011".
The length of the shortest beautiful substring is 2.
The lexicographically smallest beautiful substring with length 2 is the substring "11".
```

**Example 3**

```
Input: s = "000", k = 1
Output: ""
Explanation: There are no beautiful substrings in this example.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowMin;

/**
 * @author zhengxingxing
 * @date 2025/01/31
 */
public class ShortestAndLexicographicallySmallestBeautifulString {
    public static String shortestBeautifulSubstring(String s, int k) {
        int bestLeft = -1;  // Store the starting position of the best substring found
        int minLength = s.length() + 1;  // Initialize with length greater than possible
        int ones = 0;  // Counter for number of '1's in current window

        // Iterate through string using sliding window technique
        for (int left = 0, right = 0; right < s.length(); right++) {
            // Count '1's as we expand the window
            if (s.charAt(right) == '1') {
                ones++;
            }

            // Process window when we have exactly k ones
            while (ones == k) {
                /* First condition: Check if current window length is smaller than minLength
                 * If true, we've found a shorter valid substring
                 * Update both bestLeft and minLength to track this new shortest substring
                 * Example: if current window "11001" (length 5) is shorter than previous "100011" (length 6)
                 */
                if (right - left + 1 < minLength) {
                    bestLeft = left;
                    minLength = right - left + 1;
                }
                /* Second condition: If current window length equals minLength
                 * We need to check if current substring is lexicographically smaller
                 * bestLeft == -1: handles the first valid substring case
                 * compareTo < 0: true if current substring is lexicographically smaller
                 * Example: "01111" is lexicographically smaller than "10001" (both length 5)
                 */
                else if (right - left + 1 == minLength &&
                        (bestLeft == -1 || s.substring(left, left + minLength)
                                .compareTo(s.substring(bestLeft, bestLeft + minLength)) < 0)) {
                    bestLeft = left;
                }

                // Shrink window from left and update ones count
                if (s.charAt(left++) == '1') {
                    ones--;
                }
            }
        }

        // Return empty string if no valid substring found, otherwise return the best substring
        return bestLeft == -1 ? "" : s.substring(bestLeft, bestLeft + minLength);
    }

    public static void main(String[] args) {
        // Test case 1: Should find shortest substring with exactly 3 ones
        String s1 = "100011001";
        int k1 = 3;
        System.out.println("Test case 1 result: " + shortestBeautifulSubstring(s1, k1)); // Expected: "11001"

        // Test case 2: Should find shortest substring with exactly 2 ones
        String s2 = "1011";
        int k2 = 2;
        System.out.println("Test case 2 result: " + shortestBeautifulSubstring(s2, k2)); // Expected: "11"

        // Test case 3: Should return empty string as no substring has exactly 1 one
        String s3 = "000";
        int k3 = 1;
        System.out.println("Test case 3 result: " + shortestBeautifulSubstring(s3, k3)); // Expected: ""
    }
}

```





