---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3090. Maximum Length Substring With Two Occurrences"
date: "2025-01-11"
tags: Easy
categories:
  - "LeetCode DynamicSlidingWindow"
---


- Given a string `s`, return the **maximum** length of a substring such that it contains *at most two occurrences* of each character.

**Example 1**

```
Input: s = "bcbbbcba"
Output: 4

Explanation:
The following substring has a length of 4 and contains at most two occurrences of each character: "bcbbbcba".
```

**Example 2**

```
Input: s = "aaaa"
Output: 2

Explanation:
The following substring has a length of 2 and contains at most two occurrences of each character: "aaaa".
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/11
 */
public class MaximumLengthSubstringWithTwoOccurrences {
    public static int maximumLengthSubstring(String S) {
        char[] s = S.toCharArray(); // Convert the input string to a character array for easier access.
        int[] count = new int[26]; // Array to track the frequency of each character ('a' to 'z').
        int maxLen = 0; // Variable to store the maximum length of the substring found so far.
        int left = 0; // Left pointer of the sliding window.

        for (int right = 0; right < s.length; right++) {
            // 1. Enter window
            // Increment the frequency of the current character as it enters the window.
            count[s[right] - 'a']++;

            // 2. Maintain the condition: At most two occurrences of each character.
            // If the frequency of the current character exceeds 2, move the left pointer
            // to shrink the window until the condition is satisfied.
            while (count[s[right] - 'a'] > 2) {
                // 3. Exit window
                // Decrease the frequency of the character that is leaving the window
                // and move the left pointer one step forward.
                count[s[left] - 'a']--;
                left++;
            }

            // 4. Update the maximum length
            // Calculate the length of the current valid window and update the maxLen if needed.
            maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen; // Return the maximum length of the substring found.
    }

    public static void main(String[] args) {
        // Test cases
        System.out.println(maximumLengthSubstring("bcbbbcba")); // Output: 4
        System.out.println(maximumLengthSubstring("aaaa"));     // Output: 2
        System.out.println(maximumLengthSubstring("abcabc"));   // Output: 6
        System.out.println(maximumLengthSubstring(""));         // Output: 0
        System.out.println(maximumLengthSubstring("aabbccdd")); // Output: 8
    }
}

```





