---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "76. Minimum Window Substring"
date: "2024-02-28"
tags: Hard SlideWindow
categories:
  - "LeetCode SlideWindow"
---


- Given two strings s and t of lengths m and n respectively, return the minimum window
  substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".
- The testcases will be generated such that the answer is **unique**.

**Example 1**

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

**Example 2**

```
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.
```

**Example 3**

```
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengstars
 * @date 2024/03/30
 */
public class MinimumWindowSubstring {
    public static String minWindow(String s, String t) {
        // Array for recording the count of characters, total number of ASCII characters is 128
        int[] cnt = new int[128];

        // Record the length of target string t
        int total = t.length();

        // Calculate the number of characters in the target string t
        for (char c : t.toCharArray()) {
            cnt[c]++;
        }

        // Variable to record the starting position of the smallest window
        int from = 0;

        // Variable to record the length of the smallest window, initialized to the maximum value
        int min = Integer.MAX_VALUE;

        // Main loop, for sliding window
        for (int right = 0, left = 0, count = 0; right < s.length(); right++) {
            // If the new character added to the window exists in the target string, the counter count increases by 1
            if (--cnt[s.charAt(right)] >= 0) {
                count++;
            }

            // When the number of characters in the window meets the target string, enter the loop, trying to narrow the window
            while (count == total) {

                // If the current window length is less than the smallest window length, update the smallest window length and the starting position
                if (right - left + 1 < min) {
                    min = right - left + 1;
                    from = left;
                }

                // Try to eliminate the character on the left side of the window
                if (++cnt[s.charAt(left)] > 0) {
                    count--;
                }

                // Right shift of the left boundary of the window
                left++;
            }
        }

        // If no legal window is found, return "", otherwise, return the window string
        return (min == Integer.MAX_VALUE) ? "" : s.substring(from, from + min);
    }

    public static void main(String[] args) {
        String s1 = "ADOBECODEBANC", t1 = "ABC";
        String s2 = "a", t2 = "a";
        String s3 = "a", t3 = "aa";
        System.out.println(minWindow(s1, t1));  // BANC
        //System.out.println(minWindow(s2, t2));  // a
        //System.out.println(minWindow(s3, t3));  // ""
    }
}
```

