---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3298. Count Substrings That Can Be Rearranged to Contain a String II"
date: "2025-01-10"
tags: Hard
categories:
  - "LeetCode SlideWindow"
---

- You are given two strings `word1` and `word2`.
- A string `x` is called **valid** if `x` can be rearranged to have `word2` as a prefix.
- Return the total number of **valid** substrings of `word1`.
- **Note** that the memory limits in this problem are **smaller** than usual, so you **must** implement a solution with a *linear* runtime complexity.

**Example 1**

```
Input: nums = [90], k = 1
Output: 0
Explanation: There is one way to pick score(s) of one student:
- [90]. The difference between the highest and lowest score is 90 - 90 = 0.
The minimum possible difference is 0.
```

**Example 2**

```
Input: nums = [9,4,1,7], k = 2
Output: 2
Explanation: There are six ways to pick score(s) of two students:
- [9,4,1,7]. The difference between the highest and lowest score is 9 - 4 = 5.
- [9,4,1,7]. The difference between the highest and lowest score is 9 - 1 = 8.
- [9,4,1,7]. The difference between the highest and lowest score is 9 - 7 = 2.
- [9,4,1,7]. The difference between the highest and lowest score is 4 - 1 = 3.
- [9,4,1,7]. The difference between the highest and lowest score is 7 - 4 = 3.
- [9,4,1,7]. The difference between the highest and lowest score is 7 - 1 = 6.
The minimum possible difference is 2.
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/10
 */
public class CountSubstringsThatCanBeRearrangedToContainAStringII {
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





