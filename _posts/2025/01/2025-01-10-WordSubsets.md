---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "916. Word Subsets"
date: "2025-01-10"
tags: Medium
categories:
  - "LeetCode Array"
---


- You are given two string arrays `words1` and `words2`.
- A string `b` is a **subset** of string `a` if every letter in `b` occurs in `a` including multiplicity.
  - For example, `"wrr"` is a subset of `"warrior"` but is not a subset of `"world"`.
- A string `a` from `words1` is **universal** if for every string `b` in `words2`, `b` is a subset of `a`.
- Return an array of all the **universal** strings in `words1`. You may return the answer in **any order**.

**Example 1**

```
Input: words1 = ["amazon","apple","facebook","google","leetcode"], words2 = ["e","o"]
Output: ["facebook","google","leetcode"]
```

**Example 2**

```
Input: words1 = ["amazon","apple","facebook","google","leetcode"], words2 = ["l","e"]
Output: ["apple","google","leetcode"]
```

## Method 1

```tex
【O(n * k1 + m * k2) time | O(1) space】
```

```java
package Leetcode.Array;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/01/10
 */
public class WordSubsets {
    public static List<String> wordSubsets(String[] words1, String[] words2) {
        // Array to store the maximum frequency required for each character (a-z)
        int[] maxFreq = new int[26];

        // First key loop: Process all strings in words2
        // Purpose: Find the maximum frequency required for each character across all strings in words2
        // For example, if words2 = ["wrr", "er"], we need max frequency of 'r' which is 2 (from "wrr")
        for (String word: words2) {
            // Get frequency count of characters in current word
            int[] freq = getFrequency(word);

            // Update the maximum frequency required for each character
            // This creates a "combined frequency requirement" from all words2 strings
            for (int i = 0; i < 26; i++) {
                maxFreq[i] = Math.max(maxFreq[i], freq[i]);
            }
        }

        // List to store all universal strings found in words1
        List<String> result = new ArrayList<>();

        // Second key loop: Check each string in words1
        // Purpose: Determine which strings in words1 satisfy all character frequency requirements
        for (String word : words1) {
            // Get character frequency count of current word
            int[] freq = getFrequency(word);
            // Initially assume this word is universal
            boolean isUniversal = true;

            // Compare character frequencies
            // Check if current word has enough of each required character
            for (int i = 0; i < 26; i++) {
                // If current word doesn't have enough of any required character
                // For example: if we need 2 'r's but word only has 1 'r'
                if (freq[i] < maxFreq[i]) {
                    isUniversal = false;  // Mark as non-universal
                    break;  // No need to check further characters
                }
            }

            // If word satisfied all character frequency requirements, add it to result
            if (isUniversal) {
                result.add(word);
            }
        }

        return result;
    }

    private static int[] getFrequency(String word) {
        int[] freq = new int[26];
        for (char c: word.toCharArray()) {
            freq[c - 'a']++;
        }
        return freq;
    }

    public static void main(String[] args) {
        // Test case 1: Finding words containing both 'e' and 'o'
        String[] words1Test1 = {"amazon", "apple", "facebook", "google", "leetcode"};
        String[] words2Test1 = {"e", "o"};
        System.out.println("Test case 1 result: " + wordSubsets(words1Test1, words2Test1));

        // Test case 2: Finding words containing both 'l' and 'e'
        String[] words1Test2 = {"amazon", "apple", "facebook", "google", "leetcode"};
        String[] words2Test2 = {"l", "e"};
        System.out.println("Test case 2 result: " + wordSubsets(words1Test2, words2Test2));
    }
}

```





