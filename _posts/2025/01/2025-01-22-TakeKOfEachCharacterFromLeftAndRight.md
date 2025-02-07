---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2516. Take K of Each Character From Left and Right"
date: "2025-01-22"
tags: Medium SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowMax"
---


- You are given a string `s` consisting of the characters `'a'`, `'b'`, and `'c'` and a non-negative integer `k`. Each minute, you may take either the **leftmost** character of `s`, or the **rightmost** character of `s`.
- Return *the **minimum** number of minutes needed for you to take **at least*** `k` *of each character, or return* `-1` *if it is not possible to take* `k` *of each character.*

**Example 1**

```
Input: s = "aabaaaacaabc", k = 2
Output: 8
Explanation: 
Take three characters from the left of s. You now have two 'a' characters, and one 'b' character.
Take five characters from the right of s. You now have four 'a' characters, two 'b' characters, and two 'c' characters.
A total of 3 + 5 = 8 minutes is needed.
It can be proven that 8 is the minimum number of minutes needed.
```

**Example 2**

```
Input: s = "a", k = 1
Output: -1
Explanation: It is not possible to take one 'b' or 'c' so return -1.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/22
 */
public class TakeKOfEachCharacterFromLeftAndRight {
    public static int takeCharacters(String S, int k) {
        // Convert string to char array for better access efficiency
        char[] s = S.toCharArray();

        // Count the frequency of each character ('a', 'b', 'c')
        // cnt[0] stores count of 'a', cnt[1] for 'b', cnt[2] for 'c'
        int[] cnt = new int[3];
        for (char c : s) {
            cnt[c - 'a']++;
        }

        // Check if we have enough characters of each type
        // If any character appears less than k times, it's impossible to achieve the goal
        if (cnt[0] < k || cnt[1] < k || cnt[2] < k) {
            return -1;
        }

        // Find the longest valid substring (characters within window don't need to be taken)
        // mx: stores the maximum length of valid window
        // left: left boundary of sliding window
        int mx = 0;
        int left = 0;

        // Slide the window from left to right
        // right: right boundary of sliding window
        for (int right = 0; right < s.length; right++) {
            // Include current character in window
            // Convert character to index (0 for 'a', 1 for 'b', 2 for 'c')
            int c = s[right] - 'a';
            // Decrease count as this character is now in window (not taken)
            cnt[c]--;

            // Shrink window if we don't have enough characters outside window
            // While count of any character outside window is less than k
            while (cnt[c] < k) {
                // Add back the character at left pointer (character becomes available again)
                cnt[s[left] - 'a']++;
                // Move left pointer to shrink window
                left++;
            }

            // Update maximum window length if current window is valid
            // Window is valid when all characters outside it appear at least k times
            mx = Math.max(mx, right - left + 1);
        }

        // Final result is total length minus longest valid substring length
        // This gives us minimum number of characters we need to take
        return s.length - mx;
    }

    public static void main(String[] args) {
        // Test Case 1: Standard case with multiple occurrences
        String s1 = "aabaaaacaabc";
        int k1 = 2;
        System.out.println("Test Case 1:");
        System.out.println("Input: s = " + s1 + ", k = " + k1);
        System.out.println("Output: " + takeCharacters(s1, k1));  // Expected: 8
        System.out.println();

        // Test Case 2: Impossible case (not enough characters)
        String s2 = "a";
        int k2 = 1;
        System.out.println("Test Case 2:");
        System.out.println("Input: s = " + s2 + ", k = " + k2);
        System.out.println("Output: " + takeCharacters(s2, k2));  // Expected: -1
        System.out.println();

        // Test Case 3: Minimum valid case (exact match)
        String s3 = "abc";
        int k3 = 1;
        System.out.println("Test Case 3:");
        System.out.println("Input: s = " + s3 + ", k = " + k3);
        System.out.println("Output: " + takeCharacters(s3, k3));  // Expected: 3
        System.out.println();

        // Test Case 4: Longer string with repeated pattern
        String s4 = "aabcabcabc";
        int k4 = 2;
        System.out.println("Test Case 4:");
        System.out.println("Input: s = " + s4 + ", k = " + k4);
        System.out.println("Output: " + takeCharacters(s4, k4));  // Expected: 6

        // Test Case 5: Basic pattern repeated once
        String s5 = "abcabc";
        int k5 = 1;
        System.out.println("Test Case 5:");
        System.out.println("Input: s = " + s5 + ", k = " + k5);
        System.out.println("Output: " + takeCharacters(s5, k5));
    }
}

```





