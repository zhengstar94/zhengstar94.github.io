---
toc:
beginning: true
giscus_comments: true
layout: post
title: "3136. Valid Word"
date: "2025-07-15"
tags: Easy
categories:
- "LeetCode Math"
---


- A word is considered **valid** if:
    - It contains a **minimum** of 3 characters.
    - It contains only digits (0-9), and English letters (uppercase and lowercase).
    - It includes **at least** one **vowel**.
    - It includes **at least** one **consonant**.
- You are given a string `word`.
- Return `true` if `word` is valid, otherwise, return `false`.
- **Notes:**
    - `'a'`, `'e'`, `'i'`, `'o'`, `'u'`, and their uppercases are **vowels**.
    - A **consonant** is an English letter that is not a vowel.

**Example 1**

```
Input: word = "234Adas"

Output: true

Explanation:

This word satisfies the conditions.
```

**Example 2**

```
Input: word = "b3"

Output: false

Explanation:

The length of this word is fewer than 3, and does not have a vowel.
```

**Example 3**

```
Input: word = "a3$e"

Output: false

Explanation:

This word contains a '$' character and does not have a consonant.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @Author zhengxingxing
 * @Date 2025/07/15
 */
public class ValidWord {

    public static boolean isValid(String word) {
        // If the word has fewer than 3 characters, it is automatically invalid.
        if (word.length() < 3) {
            return false;
        }

        // Flags to track if at least one vowel and one consonant are present.
        boolean hasVowel = false;
        boolean hasConsonant = false;

        // String containing all vowels (both lowercase and uppercase).
        String vowels = "aeiouAEIOU";

        // Iterate through each character in the word.
        for (char c: word.toCharArray()) {
            // Check if the character is a letter.
            if (Character.isLetter(c)){
                // If the character is a vowel, set hasVowel to true.
                if (vowels.indexOf(c) >= 0){
                    hasVowel = true;
                } else {
                    // If the character is a letter but not a vowel, it's a consonant.
                    hasConsonant = true;
                }
            } else if(!Character.isDigit(c)){
                // If the character is neither a letter nor a digit, the word is invalid.
                return false;
            }
            // If the character is a digit, do nothing (digits are allowed).
        }

        // The word is valid only if it contains at least one vowel and one consonant.
        // Note: Use logical AND (&&) to ensure both conditions are met.
        return hasVowel && hasConsonant;
    }

    public static void main(String[] args) {
        // Test cases to verify the implementation.
        System.out.println(isValid("234Adas")); // true: meets all conditions
        System.out.println(isValid("b3"));      // false: too short and no vowel
        System.out.println(isValid("a3$e"));    // false: contains invalid character '$'
    }
}

```





