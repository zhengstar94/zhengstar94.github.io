---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3. Longest Substring Without Repeating Characters"
date: "2025-01-11"
tags: Medium SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowMax"
---


- Given a string `s`, find the length of the **longest** **substring** without repeating characters.

**Example 1**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## Method 1

```tex
【O(n)time∣O(m)space】
```

```java

package Leetcode.SlideWindow;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/03/26
 */
public class LongestSubstringWithoutRepeatingCharacters {
  public static int lengthOfLongestSubstring(String S) {
    char[] s = S.toCharArray();
    // Use array instead of HashMap to store the last position of each character
    // Array index is the character's ASCII value (0-127)
    // Array value is the last position where this character appeared
    int[] lastSeen = new int[128];

    // Initialize all positions to -1, meaning no character has been seen yet
    Arrays.fill(lastSeen, -1);

    // Variable to store the length of longest substring found so far
    int ans = 0;
    // Variable to mark the start position of current window
    int start = 0;

    for (int i = 0; i < s.length; i++) {
      // 1. Window Entry Check:
      // Check if current character causes window to shrink
      // If the last occurrence of current character is within or at the start of our window,
      // we need to move the window start to avoid repeating characters
      if (lastSeen[s[i]] >= start) {
        start = lastSeen[s[i]] + 1;  // Move start to just after the last occurrence
      }

      // 2. Update Answer:
      // Current window size is (i - start + 1)
      // Update max length if current window is longer
      ans = Math.max(ans, i - start + 1);

      // 3. Update Character Position:
      // Record the current position of character for future reference
      // This helps us detect repeating characters in future iterations
      lastSeen[s[i]] = i;
    }

    return ans;
  }

  public static void main(String[] args) {
    // Test the function with example string "abcabcbb"
    String s1 = "abcabcbb";
    System.out.println(lengthOfLongestSubstring(s1));  // Output: 3

    // Test the function with example string "bbbbb"
    String s2 = "bbbbb";
    System.out.println(lengthOfLongestSubstring(s2));  // Output: 1

    // Test the function with example string "pwwkew"
    String s3 = "pwwkew";
    System.out.println(lengthOfLongestSubstring(s3));  // Output: 3
  }
}

```

