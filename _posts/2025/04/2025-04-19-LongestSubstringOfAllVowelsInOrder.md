---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1839. Longest Substring Of All Vowels in Order"
date: "2025-04-19"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---

- A string is considered **beautiful** if it satisfies the following conditions:
  - Each of the 5 English vowels (`'a'`, `'e'`, `'i'`, `'o'`, `'u'`) must appear **at least once** in it.
  - The letters must be sorted in **alphabetical order** (i.e. all `'a'`s before `'e'`s, all `'e'`s before `'i'`s, etc.).
- For example, strings `"aeiou"` and `"aaaaaaeiiiioou"` are considered **beautiful**, but `"uaeio"`, `"aeoiu"`, and `"aaaeeeooo"` are **not beautiful**.
- Given a string `word` consisting of English vowels, return *the **length of the longest beautiful substring** of* `word`*. If no such substring exists, return* `0`.
- A **substring** is a contiguous sequence of characters in a string.

**Example 1**

```
Input: word = "aeiaaioaaaaeiiiiouuuooaauuaeiu"
Output: 13
Explanation: The longest beautiful substring in word is "aaaaeiiiiouuu" of length 13.
```

**Example 2**

```
Input: word = "aeeeiiiioooauuuaeiou"
Output: 5
Explanation: The longest beautiful substring in word is "aeiou" of length 5.
```

**Example 3**

```
Input: word = "a"
Output: 0
Explanation: There is no beautiful substring, so return 0.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/19
 */
public class LongestSubstringOfAllVowelsInOrder {

    public static int longestBeautifulSubstring(String word) {
        int n = word.length();
        // Early return if string length is less than 5 (minimum length needed for all vowels)
        if (n < 5){
            return 0;
        }

        // Track the maximum length of valid beautiful substring found
        int maxLen = 0;
        // Index for traversing the string
        int i = 0;

        // Main loop to process the entire string
        while (i < n){
            // Skip if current character is not 'a' as beautiful substring must start with 'a'
            if (word.charAt(i) != 'a'){
                i++;
                continue;
            }

            // Mark the start of potential beautiful substring
            int start = i;
            // Counter for different vowels encountered (starts with 1 for 'a')
            int vowelCount = 1;
            // Track current vowel in the sequence
            char currentVowel = 'a';
            i++;

            // Inner loop to find the length of current beautiful substring candidate
            while (i < n){
                char c = word.charAt(i);
                // Case 1: Current character is same as previous vowel
                // Example: "aa" or "ee" - continue counting length
                if (c == currentVowel){
                    i++;
                }
                // Case 2: Current character is the next vowel in sequence
                // Example: 'a' -> 'e' or 'e' -> 'i'
                else if(isNextVowel(currentVowel, c)){
                    currentVowel = c;        // Update current vowel
                    vowelCount++;            // Increment unique vowel count
                    i++;
                }
                // Case 3: Invalid sequence found, break the inner loop
                else{
                    break;
                }
            }

            // Check if we found a valid beautiful substring
            // (must contain all 5 vowels in order)
            if (vowelCount == 5) {
                maxLen = Math.max(maxLen, i - start);
            }
        }
        return maxLen;
    }
    
    private static boolean isNextVowel(char current, char next) {
        switch (current) {
            case 'a': return next == 'e';    // 'a' should be followed by 'e'
            case 'e': return next == 'i';    // 'e' should be followed by 'i'
            case 'i': return next == 'o';    // 'i' should be followed by 'o'
            case 'o': return next == 'u';    // 'o' should be followed by 'u'
            default: return false;           // Any other case is invalid
        }
    }

    public static void main(String[] args) {
        // Test case 1: Should find "aaaaeiiiiouuu" (length 13)
        String word1 = "aeiaaioaaaaeiiiiouuuooaauuaeiu";
        System.out.println("Test case 1: " + longestBeautifulSubstring(word1)); // Should output 13

        // Test case 2: Should find "aeiou" (length 5)
        String word2 = "aeeeiiiioooauuuaeiou";
        System.out.println("Test case 2: " + longestBeautifulSubstring(word2)); // Should output 5

        // Test case 3: No valid beautiful substring possible
        String word3 = "a";
        System.out.println("Test case 3: " + longestBeautifulSubstring(word3)); // Should output 0
    }
}

```





