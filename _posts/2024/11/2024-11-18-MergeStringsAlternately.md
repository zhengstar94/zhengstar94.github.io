---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1768. Merge Strings Alternately"
date: "2024-11-18"
categories:
  - "LeetCode Two Pointers"
---

- You are given two strings `word1` and `word2`. Merge the strings by adding letters in alternating order, starting with `word1`. If a string is longer than the other, append the additional letters onto the end of the merged string.
- Return *the merged string.*

**Example 1**

```
Input: word1 = "abc", word2 = "pqr"
Output: "apbqcr"
Explanation: The merged string will be merged as so:
word1:  a   b   c
word2:    p   q   r
merged: a p b q c r
```

**Example 2**

```
Input: word1 = "ab", word2 = "pqrs"
Output: "apbqrs"
Explanation: Notice that as word2 is longer, "rs" is appended to the end.
word1:  a   b 
word2:    p   q   r   s
merged: a p b q   r   s
```

**Example 3**

```
Input: word1 = "abcd", word2 = "pq"
Output: "apbqcd"
Explanation: Notice that as word1 is longer, "cd" is appended to the end.
word1:  a   b   c   d
word2:    p   q 
merged: a p b q c   d
```

## Method 1

```tex
【O(m + n) time | O(m + n) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2024/11/18
 */
public class MergeStringsAlternately {
    public static String mergeAlternately(String word1, String word2) {
        // StringBuilder is used for efficient string concatenation
        StringBuilder result = new StringBuilder();
        // Two pointers to track current position in each string
        int i = 0; // pointer for word1
        int j = 0; // pointer for word2

        // Process both strings until we reach the end of either string
        while (i < word1.length() && j < word2.length()){
            // Append current character from word1 and increment pointer
            result.append(word1.charAt(i++));
            // Append current character from word2 and increment pointer
            result.append(word2.charAt(j++));
        }

        // Append remaining characters from word1 (if any)
        while (i < word1.length()){
            result.append(word1.charAt(i++));
        }

        // Append remaining characters from word2 (if any)
        while (j < word2.length()){
            result.append(word2.charAt(j++));
        }

        // Convert StringBuilder to String and return
        return result.toString();
    }

    public static void main(String[] args) {
        // Test Case 1: Equal length strings
        String word1 = "abc";
        String word2 = "pqr";
        String result1 = mergeAlternately(word1, word2);
        System.out.println("Test Case 1 - Equal length strings:");
        System.out.println("Input: word1 = " + word1 + ", word2 = " + word2);
        System.out.println("Output: " + result1);
        System.out.println("Expected: apbqcr");
        System.out.println();

        // Test Case 2: Second string is longer
        word1 = "ab";
        word2 = "pqrs";
        String result2 = mergeAlternately(word1, word2);
        System.out.println("Test Case 2 - Second string is longer:");
        System.out.println("Input: word1 = " + word1 + ", word2 = " + word2);
        System.out.println("Output: " + result2);
        System.out.println("Expected: apbqrs");
        System.out.println();

        // Test Case 3: First string is longer
        word1 = "abcd";
        word2 = "pq";
        String result3 = mergeAlternately(word1, word2);
        System.out.println("Test Case 3 - First string is longer:");
        System.out.println("Input: word1 = " + word1 + ", word2 = " + word2);
        System.out.println("Output: " + result3);
        System.out.println("Expected: apbqcd");
        System.out.println();

        // Test Case 4: Empty string case
        word1 = "";
        word2 = "pqr";
        String result4 = mergeAlternately(word1, word2);
        System.out.println("Test Case 4 - First string is empty:");
        System.out.println("Input: word1 = " + word1 + ", word2 = " + word2);
        System.out.println("Output: " + result4);
        System.out.println("Expected: pqr");
    }
}

```
