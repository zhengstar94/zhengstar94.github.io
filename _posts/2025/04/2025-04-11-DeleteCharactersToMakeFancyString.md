---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1957. Delete Characters to Make Fancy String"
date: "2025-04-11"
tags: Easy
categories:
  - "LeetCode GroupedLoop"
---


- A **fancy string** is a string where no **three** **consecutive** characters are equal.
- Given a string `s`, delete the **minimum** possible number of characters from `s` to make it **fancy**.
- Return *the final string after the deletion*. It can be shown that the answer will always be **unique**.

**Example 1**

```
Input: s = "leeetcode"
Output: "leetcode"
Explanation:
Remove an 'e' from the first group of 'e's to create "leetcode".
No three consecutive characters are equal, so return "leetcode".
```

**Example 2**

```
Input: s = "aaabaaaa"
Output: "aabaa"
Explanation:
Remove an 'a' from the first group of 'a's to create "aabaaaa".
Remove two 'a's from the second group of 'a's to create "aabaa".
No three consecutive characters are equal, so return "aabaa".
```

**Example 3**

```
Input: s = "aab"
Output: "aab"
Explanation: No three consecutive characters are equal, so return "aab".
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/11
 */
public class DeleteCharactersToMakeFancyString {

    public static String makeFancyString(String s) {
        // Initialize StringBuilder for efficient string manipulation
        StringBuilder result = new StringBuilder();
        int n = s.length();
        int i = 0;

        // Early return for strings of length 2 or less
        // These strings are already fancy by definition
        if (n <= 2){
            return s;
        }

        // Process string using grouped loop pattern
        while (i < n){
            // Mark the start of current character group
            int start = i;
            char currentChar = s.charAt(i);

            // Find the end of current character group
            // Group consists of consecutive identical characters
            while (i < n && s.charAt(i) == currentChar){
                i++;
            }

            // Calculate the length of current character group
            int groupLength = i - start;

            // Keep at most 2 characters from each group
            // This ensures no three consecutive characters are equal
            int charsToKeep = Math.min(groupLength, 2);
            for (int j = 0; j < charsToKeep; j++) {
                result.append(currentChar);
            }
        }

        return result.toString();
    }

    public static void main(String[] args) {
        // Test Case 1: String with consecutive 'e'
        System.out.println("Test Case 1:");
        String s1 = "leeetcode";
        System.out.println("Input: s = \"" + s1 + "\"");
        System.out.println("Output: \"" + makeFancyString(s1) + "\"");

        // Test Case 2: String with multiple groups of consecutive 'a'
        System.out.println("\nTest Case 2:");
        String s2 = "aaabaaaa";
        System.out.println("Input: s = \"" + s2 + "\"");
        System.out.println("Output: \"" + makeFancyString(s2) + "\"");

        // Test Case 3: Already fancy string
        System.out.println("\nTest Case 3:");
        String s3 = "aab";
        System.out.println("Input: s = \"" + s3 + "\"");
        System.out.println("Output: \"" + makeFancyString(s3) + "\"");
    }
}

```





