---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "524. Longest Word in Dictionary through Deleting"
date: "2025-03-23"
tags: Medium TwoPointers
categories:
  - "LeetCode DoubleSeqSubsequencePointers"
---

- Given a string `s` and a string array `dictionary`, return *the longest string in the dictionary that can be formed by deleting some of the given string characters*. If there is more than one possible result, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

**Example 1**

```
Input: s= "abpoplea", dictionary = ["ale", "apple", "monkey", "plea"] 
Output: "apple"
```

**Example 2**

```
Input: s = "abpcplea", dictionary = [a,"b","c"］ 
Output: "a"
```

## Method 1

```tex
【O(n * x) time | O(1) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqSubsequencePointers;

import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/03/23
 */
public class LongestWordInDictionaryThroughDeleting {

    public static String findLongestWord(String s, List<String> dictionary) {
        // Initialize result string to store the longest valid word found
        String result = "";

        // Iterate through each word in the dictionary
        for (String word: dictionary) {
            // Check if current word is a subsequence of input string
            if (isSubsequence(s, word)){
                // Update result if current word is longer or
                // has same length but lexicographically smaller
                if (word.length() > result.length() ||
                        (word.length() == result.length() && word.compareTo(result) < 0)){
                    result = word;
                }
            }
        }
        return result;
    }
    
    public static boolean isSubsequence(String source, String target) {
        // Initialize two pointers for source and target strings
        int sourceIndex = 0;  // Pointer for source string
        int targetIndex = 0;  // Pointer for target string

        // Traverse both strings simultaneously
        while (sourceIndex < source.length() && targetIndex < target.length()){
            // If characters match, move target pointer forward
            if (source.charAt(sourceIndex) == target.charAt(targetIndex)){
                targetIndex++;
            }
            // Always move source pointer forward
            sourceIndex++;
        }
        // If we've matched all characters in target, targetIndex will equal target.length()
        return targetIndex == target.length();
    }

    public static void main(String[] args) {
        // Test Case 1: Normal case with expected output "apple"
        String s1 = "abpcplea";
        List<String> dictionary1 = Arrays.asList("ale", "apple", "monkey", "plea");
        System.out.println("Test Case 1 Result: " + findLongestWord(s1, dictionary1)); // Expected: "apple"

        // Test Case 2: Case with single character words
        String s2 = "abpcplea";
        List<String> dictionary2 = Arrays.asList("a", "b", "c");
        System.out.println("Test Case 2 Result: " + findLongestWord(s2, dictionary2)); // Expected: "a"

        // Test Case 3: Complex case with longer strings
        String s3 = "aewfafwafjlwajflwajflwafj";
        List<String> dictionary3 = Arrays.asList("apple", "ewaf", "awefawfwaf", "awef", "awefe", "ewafeffewafewf");
        System.out.println("Test Case 3 Result: " + findLongestWord(s3, dictionary3)); // Tests complex scenario
    }
}

```





