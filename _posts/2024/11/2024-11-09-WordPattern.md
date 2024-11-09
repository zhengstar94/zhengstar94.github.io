---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "290.Word Pattern"
date: "2024-11-09"
categories:
  - "LeetCode HashTable"
---

- Given a `pattern` and a string `s`, find if `s` follows the same pattern.
- Here **follow** means a full match, such that there is a bijection between a letter in `pattern` and a **non-empty** word in `s`. Specifically:
  - Each letter in `pattern` maps to **exactly** one unique word in `s`.
  - Each unique word in `s` maps to **exactly** one letter in `pattern`.
  - No two letters map to the same word, and no two words map to the same letter.

**Example 1**

```
Input: pattern = "abba", s = "dog cat cat dog"
Output: true

Explanation:

The bijection can be established as:
'a' maps to "dog".
'b' maps to "cat".
```

**Example 2**

```
Input: pattern = "abba", s = "dog cat cat fish"
Output: false
```

**Example 3**

```
Input: pattern = "aaaa", s = "dog cat cat dog"
Output: false
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2024/11/09
 */
public class WordPattern {
    public static boolean wordPattern(String pattern, String s) {
        // Split the input string into an array of words
        String[] words = s.split(" ");

        // Check if the length of pattern matches the number of words
        // If not, they can't possibly follow the same pattern
        if(pattern.length() != words.length){
            return false;
        }

        // Create two HashMaps for bidirectional mapping
        // charToWord: maps each character in pattern to its corresponding word
        Map<Character, String> charToWord = new HashMap<>();
        // wordToChar: maps each word to its corresponding character in pattern
        Map<String, Character> wordToChar = new HashMap<>();

        // Iterate through each character in pattern and its corresponding word
        for (int i = 0; i < pattern.length(); i++) {
            char c = pattern.charAt(i);
            String word = words[i];

            // Check character to word mapping
            if(charToWord.containsKey(c)){
                // If this character has been mapped before,
                // verify it maps to the same word
                if(!charToWord.get(c).equals(word)){
                    return false;  // Violation: same char maps to different words
                }
            }else{
                // If this is a new character, add the mapping
                charToWord.put(c, word);
            }

            // Check word to character mapping
            if(wordToChar.containsKey(word)){
                // If this word has been mapped before,
                // verify it maps to the same character
                if(wordToChar.get(word) != c){
                    return false;  // Violation: same word maps to different chars
                }
            }else{
                // If this is a new word, add the mapping
                wordToChar.put(word, c);
            }
        }
        // If we reach here, all mappings are valid
        return true;
    }

    public static void main(String[] args) {
        // Test Case 1: Should return true
        // Pattern "abba" matches with "dog cat cat dog"
        String pattern1 = "abba";
        String s1 = "dog cat cat dog";
        System.out.println("Test Case 1:");
        System.out.println("pattern = " + pattern1);
        System.out.println("s = " + s1);
        System.out.println("Result: " + wordPattern(pattern1, s1));
        System.out.println();

        // Test Case 2: Should return false
        // Pattern "abba" doesn't match with "dog cat cat fish"
        String pattern2 = "abba";
        String s2 = "dog cat cat fish";
        System.out.println("Test Case 2:");
        System.out.println("pattern = " + pattern2);
        System.out.println("s = " + s2);
        System.out.println("Result: " + wordPattern(pattern2, s2));
        System.out.println();

        // Test Case 3: Should return false
        // Pattern "aaaa" doesn't match with "dog cat cat dog"
        String pattern3 = "aaaa";
        String s3 = "dog cat cat dog";
        System.out.println("Test Case 3:");
        System.out.println("pattern = " + pattern3);
        System.out.println("s = " + s3);
        System.out.println("Result: " + wordPattern(pattern3, s3));
    }
}
```
