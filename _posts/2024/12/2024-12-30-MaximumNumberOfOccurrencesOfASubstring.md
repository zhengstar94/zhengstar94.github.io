---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1297. Maximum Number of Occurrences of a Substring"
date: "2024-12-30"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---


- Given a string `s`, return the maximum number of occurrences of **any** substring under the following rules:
  - The number of unique characters in the substring must be less than or equal to `maxLetters`.
  - The substring size must be between `minSize` and `maxSize` inclusive.

**Example 1**

```
Input: s = "aababcaab", maxLetters = 2, minSize = 3, maxSize = 4
Output: 2
Explanation: Substring "aab" has 2 occurrences in the original string.
It satisfies the conditions, 2 unique letters and size 3 (between minSize and maxSize).
```

**Example 2**

```
Input: s = "aaaa", maxLetters = 1, minSize = 3, maxSize = 3
Output: 2
Explanation: Substring "aaa" occur 2 times in the string. It can overlap.
```

**Example 3**

```
Input: s = "abcde", maxLetters = 2, minSize = 3, maxSize = 3
Output: 0
```

## Method 1

```tex
【O(n * minSize) time | O(n) space】
```

```java
package Leetcode.SlideWindow;

import java.util.Collections;
import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2024/12/30
 */
public class MaximumNumberOfOccurrencesOfASubstring {
    public static int maxFreq(String s, int maxLetters, int minSize, int maxSize) {
        // If the string is null or its length is less than the minimum size, return 0
        if(s == null || s.length() < minSize){
            return 0;
        }

        // A map to store the frequency of each valid substring
        Map<String, Integer> count = new HashMap<>();

        // An array to track the count of characters in the current sliding window
        int[] charCount = new int[128];

        // The number of unique characters in the current window
        int uniqueChars = 0;

        // Step 1: Initialize the first window by processing the first (minSize-1) characters
        for (int i = 0; i < minSize - 1; i++) {
            // Get the character at the current index
            char c = s.charAt(i);

            // Increment the count for this character and check if it is a new unique character
            if(charCount[c]++ == 0){
                uniqueChars++;  // If it's the first occurrence of this character, increment uniqueChars
            }
        }

        // Step 2: Start sliding the window across the string
        for (int i = minSize - 1; i < s.length(); i++){
            // Add the current character to the sliding window
            char addChar = s.charAt(i);
            if(charCount[addChar]++ == 0){
                uniqueChars++;  // If this character is new to the window, increment uniqueChars
            }

            // If the number of unique characters in the window is within the allowed limit
            if(uniqueChars <= maxLetters){
                // Extract the current substring (from index i-minSize+1 to i+1) and add it to the map
                String subStr = s.substring(i - minSize + 1, i + 1);
                // Update the frequency of this substring in the map
                count.merge(subStr, 1, Integer::sum);
            }

            // Step 3: Remove the character that is sliding out of the window from the left
            char removeChar = s.charAt(i - minSize + 1);
            // Decrement the count of the character and check if it’s now no longer in the window
            if (--charCount[removeChar] == 0) {
                uniqueChars--;  // If the character's count reaches 0, decrement uniqueChars
            }
        }

        // Step 4: Return the highest frequency of any valid substring, or 0 if no valid substrings were found
        return count.isEmpty() ? 0 : Collections.max(count.values());
    }

    public static void main(String[] args) {
        // Example input string and parameters
        String s = "aababcaab";
        int maxLetters = 2; // Maximum allowed distinct characters in each substring
        int minSize = 3;    // Minimum length of substrings to consider
        int maxSize = 4;    // Maximum length of substrings (not used in this solution)

        // Print the input parameters
        System.out.println("Input String: " + s);
        System.out.println("maxLetters: " + maxLetters);
        System.out.println("minSize: " + minSize);
        System.out.println("maxSize: " + maxSize);
        System.out.println("\nProcessing...\n");

        // Call the method and print the result
        int result = maxFreq(s, maxLetters, minSize, maxSize);
        System.out.println("\nFinal Result: " + result);
    }
}

```





