---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "438. Find All Anagrams in a String"
date: "2025-01-03"
tags: Medium SlideWindow
categories:
  - "LeetCode SlideWindow"
---


- Given two strings `s` and `p`, return an array of all the start indices of `p`'s anagrams in `s`. You may return the answer in **any order**.

**Example 1**

```
Input: s = "cbaebabacd", p = "abc"
Output: [0,6]
Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

**Example 2**

```
Input: s = "abab", p = "ab"
Output: [0,1,2]
Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

## Method 1

```tex
【O(n + m) time | O(k) space】
```

```java
package Leetcode.SlideWindow;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/01/03
 */
public class FindAllAnagramsInAString {
    
    public static List<Integer> findAnagrams(String s, String p) {
        // Result list to store starting indices of anagrams
        List<Integer> result = new ArrayList<>();
        int lenS = s.length();
        int lenP = p.length();

        // If the length of s is less than p, no anagrams are possible
        if (lenS < lenP) {
            return result;
        }

        // Arrays to store the frequency of characters in p and the sliding window in s
        int[] countP = new int[26];
        int[] countWindow = new int[26];

        // Populate the frequency array for string p
        for (int i = 0; i < lenP; i++) {
            countP[p.charAt(i) - 'a']++;
        }

        // Sliding window over string s
        for (int i = 0; i < lenS; i++) {
            // Add the current character to the sliding window
            countWindow[s.charAt(i) - 'a']++;

            // Check if the window size exceeds lenP
            if (i >= lenP - 1) {
                // If the character frequencies match, an anagram is found
                if (matches(countP, countWindow)) {
                    // Add the starting index of the anagram to the result list
                    result.add(i - lenP + 1);
                }

                // Remove the leftmost character from the sliding window
                countWindow[s.charAt(i - lenP + 1) - 'a']--;
            }
        }

        return result;
    }
    
    private static boolean matches(int[] count1, int[] count2) {
        for (int i = 0; i < 26; i++) {
            if (count1[i] != count2[i]) {
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        // Test cases
        System.out.println(findAnagrams("cbaebabacd", "abc")); // Output: [0, 6]
        System.out.println(findAnagrams("abab", "ab"));        // Output: [0, 1, 2]
        System.out.println(findAnagrams("a", "a"));            // Output: [0]
        System.out.println(findAnagrams("a", "b"));            // Output: []
    }
}

```





