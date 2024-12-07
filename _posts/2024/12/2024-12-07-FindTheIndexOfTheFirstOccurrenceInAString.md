---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "28. Find the Index of the First Occurrence in a String"
date: "2024-12-07"
tags: Easy
categories:
  - "LeetCode String"
---

- Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

**Example 1**

```
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.
```

**Example 2**

```
Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.
```

## Method 1

```tex
【O(n * m) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2024/12/07
 */
public class FindTheIndexOfTheFirstOccurrenceInAString {
    public static int strStr(String haystack, String needle) {
        // Get the lengths of the haystack and needle
        int n = haystack.length();
        int m = needle.length();

        // Special case: if needle is empty, return 0
        if(m == 0){
            return 0;
        }

        // Iterate through possible starting positions in haystack
        // Stop at n - m to prevent index out of bounds
        for (int i = 0; i <= n - m ; i++) {

            // Index used to iterate through the characters of the needle (substring)
            // Helps track the current character position during the matching process
            // Resets to 0 for each new potential starting position in the haystack
            int j;

            // Compare characters in the current window
            for (j = 0; j < m; j++) {
                // If characters don't match, break the inner loop
                if (haystack.charAt(i + j) != needle.charAt(j)) {
                    break;
                }
            }

            // If all characters in the window match the needle
            if (j == m) {
                // Return the starting index of the match
                return i;
            }
        }

        // If no match is found after checking all possible positions
        return -1;
    }

    public static void main(String[] args) {
        // Test Case 1: Standard matching
        String haystack1 = "sadbutsad";
        String needle1 = "sad";
        System.out.println("Test Case 1:");
        System.out.println("Haystack: " + haystack1);
        System.out.println("Needle: " + needle1);
        System.out.println("Match Index: " + strStr(haystack1, needle1));
        System.out.println();

        // Test Case 2: No matching substring
        String haystack2 = "leetcode";
        String needle2 = "leeto";
        System.out.println("Test Case 2:");
        System.out.println("Haystack: " + haystack2);
        System.out.println("Needle: " + needle2);
        System.out.println("Match Index: " + strStr(haystack2, needle2));
        System.out.println();

        // Test Case 3: Empty needle
        String haystack3 = "hello";
        String needle3 = "";
        System.out.println("Test Case 3:");
        System.out.println("Haystack: " + haystack3);
        System.out.println("Needle: " + needle3);
        System.out.println("Match Index: " + strStr(haystack3, needle3));
        System.out.println();

        // Test Case 4: Multiple possible matches, return the first
        String haystack4 = "mississippi";
        String needle4 = "issip";
        System.out.println("Test Case 4:");
        System.out.println("Haystack: " + haystack4);
        System.out.println("Needle: " + needle4);
        System.out.println("Match Index: " + strStr(haystack4, needle4));
    }
}

```





