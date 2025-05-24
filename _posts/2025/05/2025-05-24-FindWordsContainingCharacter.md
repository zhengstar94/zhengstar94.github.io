---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2942. Find Words Containing Character"
date: "2025-05-24"
tags: Easy
categories:
  - "LeetCode Array"
---

- You are given a **0-indexed** array of strings `words` and a character `x`.
- Return *an **array of indices** representing the words that contain the character* `x`.
- **Note** that the returned array may be in **any** order.

**Example 1**

```
Input: words = ["leet","code"], x = "e"
Output: [0,1]
Explanation: "e" occurs in both words: "leet", and "code". Hence, we return indices 0 and 1.
```

**Example 2**

```
Input: words = ["abc","bcd","aaaa","cbc"], x = "a"
Output: [0,2]
Explanation: "a" occurs in "abc", and "aaaa". Hence, we return indices 0 and 2.
```

**Example 3**

```
Input: words = ["abc","bcd","aaaa","cbc"], x = "z"
Output: []
Explanation: "z" does not occur in any of the words. Hence, we return an empty array.
```

## Method 1

```tex
【O(n × m) time | O(k) space】
```

```java
package Leetcode.Array;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @Author zhengxingxing
 * @Date 2025/05/24
 */
public class FindWordsContainingCharacter {
    
    public static List<Integer> findWordsContaining(String[] words, char x) {
        // Initialize result list to store indices of words containing the character
        List<Integer> result = new ArrayList<>();

        // Iterate through each word in the array with index
        for (int i = 0; i < words.length; i++) {
            // Check if current word contains the target character
            // indexOf() returns -1 if character is not found, otherwise returns the first occurrence index
            if (words[i].indexOf(x) != -1) {
                // Add the current index to result list if character is found
                result.add(i);
            }
        }

        // Return the list of indices containing the target character
        return result;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Basic case with character 'e' found in both words
        String[] words1 = {"leet", "code"};
        char x1 = 'e';
        List<Integer> result1 = findWordsContaining(words1, x1);
        System.out.println("Test Case 1:");
        System.out.println("Input: words = " + Arrays.toString(words1) + ", x = '" + x1 + "'");
        System.out.println("Output: " + result1);
        System.out.println("Expected: [0, 1]");
        System.out.println();

        // Test Case 2: Character 'a' found in first and third words
        String[] words2 = {"abc", "bcd", "aaaa", "cbc"};
        char x2 = 'a';
        List<Integer> result2 = findWordsContaining(words2, x2);
        System.out.println("Test Case 2:");
        System.out.println("Input: words = " + Arrays.toString(words2) + ", x = '" + x2 + "'");
        System.out.println("Output: " + result2);
        System.out.println("Expected: [0, 2]");
        System.out.println();

        // Test Case 3: Character 'z' not found in any word - should return empty list
        String[] words3 = {"abc", "bcd", "aaaa", "cbc"};
        char x3 = 'z';
        List<Integer> result3 = findWordsContaining(words3, x3);
        System.out.println("Test Case 3:");
        System.out.println("Input: words = " + Arrays.toString(words3) + ", x = '" + x3 + "'");
        System.out.println("Output: " + result3);
        System.out.println("Expected: []");
        System.out.println();

        // Additional Test Case: Character 'o' found in multiple words
        String[] words4 = {"hello", "world", "java", "programming"};
        char x4 = 'o';
        List<Integer> result4 = findWordsContaining(words4, x4);
        System.out.println("Additional Test Case:");
        System.out.println("Input: words = " + Arrays.toString(words4) + ", x = '" + x4 + "'");
        System.out.println("Output: " + result4);
        System.out.println("Expected: [0, 1, 3]");
    }
}
```





