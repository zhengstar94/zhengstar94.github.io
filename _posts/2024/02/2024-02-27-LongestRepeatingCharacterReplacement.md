---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "424. Longest Repeating Character Replacement"
date: "2024-02-27"
tags: Medium SlideWindow
categories:
  - "LeetCode SlideWindow"
---


- You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.
- Return *the length of the longest substring containing the same letter you can get after performing the above operations*.

**Example 1**

```
Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.
```

**Example 2**

```
Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
There may exists other ways to achieve this answer too.
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengstars
 * @date 2024/03/27
 */
public class LongestRepeatingCharacterReplacement {

    // Main function to calculate the longest repeating character with replacement
    public static int characterReplacement(String s, int k) {
        // Convert the input string to character array
        char[] cur = s.toCharArray();

        // Declare an array to store counts of each letter
        int[] map = new int[26];
        // Get the length of the input string
        int n = s.length();
        // Initialize the max length
        int ans = 0;
        // Initialize the left pointer of the sliding window
        int left = 0;

        // Start sliding window from the left to the right
        for (int right = 0; right < n; right++) {
            // Calculate the index of current character in the map array
            int rider = cur[right] - 'A';
            // Increase the count of current character
            map[rider]++;
            // Update the max count
            ans = Math.max(ans, map[rider]);

            // If the length of the current window minus max count is larger than k
            if (right - left + 1 > ans + k) {
                // Decrease the count of the character at left pointer
                map[cur[left] - 'A']--;
                // Move the left pointer to the right by one unit
                left++;
            }
        }
        // Return the max length of the substring
        return cur.length - left;
    }

    public static void main(String[] args) {
        // Test case 1
        String s1 = "ABAB";
        int k1 = 2;
        // Print the result of the test case 1
        System.out.println(characterReplacement(s1, k1));

        // Test case 2
        String s2 = "AABABBA";
        int k2 = 2;
        // Print the result of the test case 2
        System.out.println(characterReplacement(s2, k2));
    }
}
```

