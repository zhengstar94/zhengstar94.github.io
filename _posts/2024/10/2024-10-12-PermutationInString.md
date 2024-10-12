---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "567.Permutation in String"
date: "2024-10-12"
categories:
  - "LeetCode SlideWindow"
---

- Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.
- In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.


**Example 1**

```
Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").
```

**Example 2**

```
Input: s1 = "ab", s2 = "eidboaoo"
Output: false
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Stack;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/10/12
 */
public class PermutationInString {
    public static boolean checkInclusion(String s1, String s2) {
        // If s1 is longer than s2, it's impossible for s1's permutation to be in s2
        if (s1.length() > s2.length()) {
            return false;
        }

        // Arrays to store character frequencies
        int[] count1 = new int[26];
        int[] count2 = new int[26];

        // Count the frequency of characters in s1
        for (char c : s1.toCharArray()) {
            count1[c - 'a']++;
        }

        // Sliding window approach
        for (int i = 0; i < s2.length(); i++) {
            // Add the current character to count2
            count2[s2.charAt(i) - 'a']++;

            // IMPORTANT: Remove the leftmost character of the previous window
            if (i >= s1.length()) {
                // i - s1.length() gives the index of the character to be removed
                // This ensures that we maintain a window of size s1.length()
                count2[s2.charAt(i - s1.length()) - 'a']--;
            }

            // Check if the current window is a permutation of s1
            if (Arrays.equals(count1, count2)) {
                return true;
            }
        }

        // If no permutation is found
        return false;
    }

    // Test cases
    public static void main(String[] args) {

        // Test case 1
        String s1 = "ab";
        String s2 = "eidbaooo";
        System.out.println("Test case 1: " + checkInclusion(s1, s2)); // Expected: true

        // Test case 2
        s1 = "ab";
        s2 = "eidboaoo";
        System.out.println("Test case 2: " + checkInclusion(s1, s2)); // Expected: false

        // Test case 3
        s1 = "adc";
        s2 = "dcda";
        System.out.println("Test case 3: " + checkInclusion(s1, s2)); // Expected: true

        // Test case 4
        s1 = "hello";
        s2 = "ooolleoooleh";
        System.out.println("Test case 4: " + checkInclusion(s1, s2)); // Expected: false

        // Test case 5
        s1 = "abcd";
        s2 = "abcd";
        System.out.println("Test case 5: " + checkInclusion(s1, s2)); // Expected: true
    }
}

```





