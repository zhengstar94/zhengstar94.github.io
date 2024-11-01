---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "953.Verifying an Alien Dictionary"
date: "2024-11-01"
categories:
  - "LeetCode HashTable"
---

- In an alien language, surprisingly, they also use English lowercase letters, but possibly in a different `order`. The `order` of the alphabet is some permutation of lowercase letters.
- Given a sequence of `words` written in the alien language, and the `order` of the alphabet, return `true` if and only if the given `words` are sorted lexicographically in this alien language.

**Example 1**

```
Input: words = [ "hello","leetcode" ], order = "hlabcdefgijkmnopqrstuvwxyz"
Output: true
Explanation: As 'h' comes before 'l' in this language, then the sequence is sorted.
```

**Example 2**

```
Input: words = [ "word","world","row" ], order = "worldabcefghijkmnpqstuvxyz"
Output: false
Explanation: As 'd' comes after 'l' in this language, then words[0] > words[1], hence the sequence is unsorted.
```

**Example 3**

```
Input: words = [ "apple","app" ], order = "abcdefghijklmnopqrstuvwxyz"
Output: false
Explanation: The first three characters "app" match, and the second string is shorter (in size.) According to lexicographical rules "apple" > "app", because 'l' > '∅', where '∅' is defined as the blank character which is less than any other character (More info).
```

## Method 1

```tex
【O(m * n) time | O(1) space】
```

```java
package Leetcode.HashTable;

/**
 * @author zhengstars
 * @date 2024/11/01
 */
public class VerifyingAnAlienDictionary {
    // Character mapping array to store the order of characters in alien dictionary
    // Index represents the character (a-z), value represents its position in alien order
    private static int[] charMap;

    public static boolean isAlienSorted(String[] words, String order) {
        // Initialize character mapping array (size 26 for lowercase letters a-z)
        charMap = new int[26];

        // Build character mapping: store each character's position in the alien order
        // For example: if order = "hlabcd", then h->0, l->1, a->2, b->3, etc.
        for (int i = 0; i < order.length(); i++) {
            charMap[order.charAt(i) - 'a'] = i;
        }

        // Compare adjacent words pairwise
        // If any pair is in wrong order, return false
        for (int i = 0; i < words.length - 1; i++) {
            if(compare(words[i], words[i + 1]) > 0) {
                return false;  // Current word is greater than next word (wrong order)
            }
        }

        return true;  // All pairs are in correct order
    }

    private static int compare(String word1, String word2) {
        // Get lengths of both words
        int len1 = word1.length();
        int len2 = word2.length();

        // Find the length of shorter word to avoid ArrayIndexOutOfBounds
        int minLen = Math.min(len1, len2);

        // Compare characters at each position
        for (int i = 0; i < minLen; i++) {
            char c1 = word1.charAt(i);
            char c2 = word2.charAt(i);
            if (c1 != c2) {
                // If characters differ, compare their positions in alien order
                // charMap[c1-'a'] gives position of c1 in alien order
                return charMap[c1 - 'a'] - charMap[c2 - 'a'];
            }
        }

        // If all characters match up to minLen, longer word should come after
        // Example: "app" should come before "apple"
        return len1 - len2;
    }

    public static void main(String[] args) {
        // Test Case 1: Basic sorting with modified alphabet
        String[] words1 = {"hello", "leetcode"};
        String order1 = "hlabcdefgijkmnopqrstuvwxyz";
        System.out.println("Test Case 1:");
        System.out.println("Expected: true");
        System.out.println("Actual: " + isAlienSorted(words1, order1));
        System.out.println();

        // Test Case 2: Words in wrong order
        String[] words2 = {"word", "world", "row"};
        String order2 = "worldabcefghijkmnpqstuvxyz";
        System.out.println("Test Case 2:");
        System.out.println("Expected: false");
        System.out.println("Actual: " + isAlienSorted(words2, order2));
        System.out.println();

        // Test Case 3: Prefix case (longer word before shorter prefix)
        String[] words3 = {"apple", "app"};
        String order3 = "abcdefghijklmnopqrstuvwxyz";
        System.out.println("Test Case 3:");
        System.out.println("Expected: false");
        System.out.println("Actual: " + isAlienSorted(words3, order3));
        System.out.println();

        // Test Case 4: Single character difference
        String[] words4 = {"aa", "ab"};
        String order4 = "abcdefghijklmnopqrstuvwxyz";
        System.out.println("Test Case 4:");
        System.out.println("Expected: true");
        System.out.println("Actual: " + isAlienSorted(words4, order4));
        System.out.println();

        // Test Case 5: Identical words
        String[] words5 = {"app", "app"};
        String order5 = "abcdefghijklmnopqrstuvwxyz";
        System.out.println("Test Case 5:");
        System.out.println("Expected: true");
        System.out.println("Actual: " + isAlienSorted(words5, order5));
    }
}

```





