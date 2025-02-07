---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "(Review)30. Substring with Concatenation of All Words"
date: "2025-01-04"
tags: Hard Review SlideWindow
categories:
  - "LeetCode SlideWindow"
---

- You are given a string `s` and an array of strings `words`. All the strings of `words` are of **the same length**.
- A **concatenated string** is a string that exactly contains all the strings of any permutation of `words` concatenated.
  - For example, if `words = ["ab","cd","ef"]`, then `"abcdef"`, `"abefcd"`, `"cdabef"`, `"cdefab"`, `"efabcd"`, and `"efcdab"` are all concatenated strings. `"acdbef"` is not a concatenated string because it is not the concatenation of any permutation of `words`.
- Return an array of *the starting indices* of all the concatenated substrings in `s`. You can return the answer in **any order**.

**Example 1**

```
Input: s = "barfoothefoobarman", words = ["foo","bar"]
Output: [0,9]

Explanation:

The substring starting at 0 is "barfoo". It is the concatenation of ["bar","foo"] which is a permutation  of words.
The substring starting at 9 is "foobar". It is the concatenation of ["foo","bar"] which is a permutation  of words.
```

**Example 2**

```
Input: s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]
Output: []

Explanation:

There is no concatenated substring.
```

**Example 3**

```
Input: s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]
Output: [6,9,12]

Explanation:

The substring starting at 6 is "foobarthe". It is the concatenation of ["foo","bar","the"].
The substring starting at 9 is "barthefoo". It is the concatenation of ["bar","the","foo"].
The substring starting at 12 is "thefoobar". It is the concatenation of ["the","foo","bar"].
```

## Method 1

```tex
【O(s.length()) time | O(words.length) space】
```

```java
package Leetcode.SlideWindow;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2025/01/04
 */
public class SubstringWithConcatenationOfAllWords {
    public static List<Integer> findSubstring(String s, String[] words) {
        // List to store the final result, records the starting indices of substrings that meet the condition
        List<Integer> result = new ArrayList<>();

        // Basic input checks, return empty result if input is null or empty
        if (s == null || s.length() == 0 || words == null || words.length == 0) {
            return result;
        }

        // Get basic information
        int wordLen = words[0].length();    // Length of each word
        int wordCount = words.length;       // Total number of words
        int totalLen = wordLen * wordCount; // Total length of all concatenated words

        // If the original string's length is smaller than the required total length, it can't match, return empty result
        if (s.length() < totalLen) {
            return result;
        }

        // Step 1: Create a HashMap to count how many times each word should appear in the words array
        // Key is the word, value is how many times the word should appear
        Map<String, Integer> wordMap = new HashMap<>();
        for (String word : words) {
            wordMap.put(word, wordMap.getOrDefault(word, 0) + 1);
        }

        // Step 2: Iterate over all possible starting positions
        // Since the length of each word is fixed, only need to iterate over wordLen offsets
        // For example, if the word length is 3, we only need to start from indices 0, 1, 2 to cover all possibilities
        for (int i = 0; i < wordLen; i++) {
            int left = i;                              // Left boundary of the sliding window
            int count = 0;                            // The number of matched words in the current window
            Map<String, Integer> curMap = new HashMap<>();  // Count of words in the current window

            // Step 3: Use sliding window, each time move by one word length
            for (int j = i; j <= s.length() - wordLen; j += wordLen) {
                // Get the word at the current position
                String word = s.substring(j, j + wordLen);

                // If the word is one of the target words (exists in the words array)
                if (wordMap.containsKey(word)) {
                    // Add the word to the count in the current window
                    curMap.put(word, curMap.getOrDefault(word, 0) + 1);
                    count++; // Increase the number of matched words

                    // If the current word appears more than required, shrink the left boundary of the window
                    // Remove the leftmost word until the count of the current word is within the required limit
                    while (curMap.get(word) > wordMap.get(word)) {
                        String leftWord = s.substring(left, left + wordLen);
                        curMap.put(leftWord, curMap.get(leftWord) - 1);
                        count--;
                        left += wordLen;
                    }

                    // If all words are found (matching the required count)
                    if (count == wordCount) {
                        result.add(left); // Add the current valid starting index to the result

                        // Remove the leftmost word and continue searching for the next match
                        // This allows finding all possible matching positions
                        String leftWord = s.substring(left, left + wordLen);
                        curMap.put(leftWord, curMap.get(leftWord) - 1);
                        count--;
                        left += wordLen;
                    }
                } else {
                    // If a word is encountered that is not in the target word list
                    // Reset all states and start matching from the next position
                    curMap.clear();
                    count = 0;
                    left = j + wordLen;
                }
            }
        }
        return result;
    }

    public static void main(String[] args) {
        // Test case 1: Basic case
        // "barfoo" and "foobar" are valid substrings
        String s1 = "barfoothefoobarman";
        String[] words1 = {"foo","bar"};
        System.out.println("Test Case 1: " + findSubstring(s1, words1)); // Expected output: [0, 9]

        // Test case 2: Repeated characters
        // Test handling of repeated words
        String s2 = "aaaaaaaa";
        String[] words2 = {"aa","aa"};
        System.out.println("Test Case 2: " + findSubstring(s2, words2));

        // Test case 3: No matches
        // Test case where no matches are found
        String s3 = "wordgoodgoodgoodbestword";
        String[] words3 = {"word","good","best","good"};
        System.out.println("Test Case 3: " + findSubstring(s3, words3)); // Expected output: []
    }
}

```





