---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3297. Count Substrings That Can Be Rearranged to Contain a String I"
date: "2025-01-09"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---


- You are given two strings `word1` and `word2`.
- A string `x` is called **valid** if `x` can be rearranged to have `word2` as a prefix.
- Return the total number of **valid** substrings of `word1`.

**Example 1**

```
Input: word1 = "bcca", word2 = "abc"
Output: 1

Explanation:

The only valid substring is "bcca" which can be rearranged to "abcc" having "abc" as a prefix.
```

**Example 2**

```
Input: word1 = "abcabc", word2 = "abc"
Output: 10

Explanation:

All the substrings except  substrings of size 1 and size 2 are valid.
```

**Example 3**

```
Input: word1 = "abcabc", word2 = "aaabc"

Output: 0
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/09
 */
public class CountSubstringsThatCanBeRearrangedToContainAStringI {
    public static long validSubstringCount(String word1, String word2) {
        // Early return if word1 is shorter than word2, as no valid substring possible
        if (word1.length() < word2.length()) {
            return 0;
        }

        // Convert strings to char arrays for efficient access
        char[] s = word1.toCharArray();
        char[] t = word2.toCharArray();

        // Array to track the difference in character frequencies
        // diff[i] > 0 means we need more of character i
        // diff[i] = 0 means we have exactly enough of character i
        // diff[i] < 0 means we have excess of character i
        int[] diff = new int[26];

        // Initialize diff array with character frequencies from word2
        for (char c : t) {
            diff[c - 'a']++;
        }

        // Count how many characters we still need
        // less represents the number of unique characters we still need
        int less = 0;
        for (int d : diff) {
            if (d > 0) {
                less++;
            }
        }

        // Variables to track results and window
        long ans = 0;     // Total count of valid substrings
        int left = 0;     // Left boundary of sliding window

        // Process each character in word1
        for (char c : s) {
            // Add current character to window by decreasing its needed count
            diff[c - 'a']--;

            // If after adding this character, its frequency matches exactly what we need
            // (diff becomes 0), we have one less character to worry about
            if (diff[c - 'a'] == 0) {
                less--;
            }

            // When less == 0, we have all characters we need
            // Try to minimize the window by removing characters from left
            while (less == 0) {
                // Get the character we're about to remove
                char outChar = s[left++];

                // If this character's count was exactly what we needed (diff == 0)
                // removing it will make it deficient, so increase less
                if (diff[outChar - 'a'] == 0) {
                    less++;
                }
                // Update diff array as we remove the character
                diff[outChar - 'a']++;
            }

            // Add left to answer - this is crucial!
            // left represents how many valid substrings end at the current position
            // because we can start the substring at any position from 0 to left-1
            ans += left;
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test case 1: Expect 1 valid substring
        String word1_1 = "bcca";
        String word2_1 = "abc";
        System.out.println("Test case 1 result: " + validSubstringCount(word1_1, word2_1)); // Expected: 1

        // Test case 2: Expect 10 valid substrings
        String word1_2 = "abcabc";
        String word2_2 = "abc";
        System.out.println("Test case 2 result: " + validSubstringCount(word1_2, word2_2)); // Expected: 10

        // Test case 3: Expect 0 valid substrings
        String word1_3 = "abcabc";
        String word2_3 = "aaabc";
        System.out.println("Test case 3 result: " + validSubstringCount(word1_3, word2_3)); // Expected: 0
    }
}

```





