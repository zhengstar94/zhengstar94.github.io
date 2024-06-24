---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3. Longest Substring Without Repeating Characters"
date: "2024-02-26"
categories:
  - "LeetCode SlideWindow"
---

# LeetCode 3. Longest Substring Without Repeating Characters 

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
【O(n)time∣O(n)space】
```

```java
package Leetcode.SlideWindow;

import java.util.HashMap;

/**
 * @author zhengstars
 * @date 2024/03/26
 */
public class LongestSubstringWithoutRepeatingCharacters {
    public static int lengthOfLongestSubstring(String s) {
        // We start by initializing a HashMap to keep track of the characters we have encountered
        HashMap<Character, Integer> map = new HashMap<>();

        // 'start' and 'end' pointers represent the current window
        // 'length' holds the maximum length of substring without repeating characters we have found till now.
        int start = 0, end = 0, length = 0;

        // We expand the window by incrementing 'end' and move 'start' as needed
        while (end < s.length()){
            // The current character at the 'end' of the window
            char c = s.charAt(end);

            // If character 'c' has already been encountered, move 'start' pointer to the next position from its previous occurrence
            if(map.containsKey(c)){
                start = Math.max(start, map.get(c) + 1);
            }

            // Update the position of character 'c'
            map.put(c, end);

            // Update the length of the longest substring without repeating characters
            length = Math.max(length, end-start+1);

            // Expand the window
            end++;
        }
        // Finally, return the length of longest substring without repeating characters
        return length;
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

