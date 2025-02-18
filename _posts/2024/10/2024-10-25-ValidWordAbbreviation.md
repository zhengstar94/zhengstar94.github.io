---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "408.Valid Word Abbreviation"
date: "2024-10-25"
categories:
  - "LeetCode TwoPointers"
---

- Given a non-empty string s and an abbreviation abbr, return whether the string matches with the given abbreviation.

- A string such as "word" contains only the following valid abbreviations:

  ```
  ["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
  ```

  Notice that only the above abbreviations are valid abbreviations of the string "word". Any other string is not a valid abbreviation of "word".

  Note: Assume s contains only lowercase letters and abbr contains only lowercase letters and digits.

**Example 1**

```
Given s = "internationalization", abbr = "i12iz4n":
Return true.
```

**Example 2**

```
Given s = "apple", abbr = "a2e":
Return false.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengstars
 * @date 2024/10/25
 */
public class ValidWordAbbreviation {
    public static boolean validWordAbbreviation(String word, String abbr) {
        // Handle edge cases: if either string is null
        if(word == null || abbr == null){
            return false;
        }

        // Initialize two pointers:
        // i: points to the current position in original word
        // j: points to the current position in abbreviation
        int i = 0;
        int j = 0;

        // Continue while both pointers are within their respective strings
        while (i < word.length() && j < abbr.length()){
            // If current character in abbreviation is a digit
            if(Character.isDigit(abbr.charAt(j))){
                // Leading zeros are not allowed in valid abbreviation
                if(abbr.charAt(j) == '0'){
                    return false;
                }

                // Variable to store the complete number when encountering multiple consecutive digits
                int num = 0;
                // Process continuous digits (e.g., "12" in "i12iz4n")
                // For example: if we encounter "12":
                // First iteration: num = 0 * 10 + 1 = 1
                // Second iteration: num = 1 * 10 + 2 = 12
                while (j < abbr.length() && Character.isDigit(abbr.charAt(j))){
                    // Convert char to int and accumulate the number
                    num = num * 10 + (abbr.charAt(j) - '0');
                    // Move to next digit
                    j++;
                }

                // Skip 'num' characters in the original word
                i += num;
            } else {
                // If current characters don't match or we've exceeded word length
                if(i >= word.length() || word.charAt(i) != abbr.charAt(j)){
                    return false;
                }
                // Move both pointers for character comparison
                i++;
                j++;
            }
        }

        // Valid only if both strings are completely traversed
        return i == word.length() && j == abbr.length();
    }

    public static void main(String[] args) {
        // Test case 1: Expected to return true
        // "internationalization" -> "i12iz4n"
        // where 12 represents 12 characters and 4 represents 4 characters
        String s1 = "internationalization";
        String abbr1 = "i12iz4n";
        System.out.println("Test case 1: " + validWordAbbreviation(s1, abbr1)); // Should print true

        // Test case 2: Expected to return false
        // "apple" cannot be abbreviated as "a2e"
        // because the middle part "ppl" is 3 characters, not 2
        String s2 = "apple";
        String abbr2 = "a2e";
        System.out.println("Test case 2: " + validWordAbbreviation(s2, abbr2)); // Should print false
    }
}

```





