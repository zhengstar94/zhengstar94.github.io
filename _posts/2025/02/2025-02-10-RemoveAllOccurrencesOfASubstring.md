---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1910. Remove All Occurrences of a Substring"
date: "2025-02-10"
tags: Medium
categories:
  - "LeetCode String"
---



- Given two strings `s` and `part`, perform the following operation on `s` until **all** occurrences of the substring `part` are removed:
  - Find the **leftmost** occurrence of the substring `part` and **remove** it from `s`.
- Return `s` *after removing all occurrences of* `part`.
- A **substring** is a contiguous sequence of characters in a string.

**Example 1**

```
Input: s = "daabcbaabcbc", part = "abc"
Output: "dab"
Explanation: The following operations are done:
- s = "daabcbaabcbc", remove "abc" starting at index 2, so s = "dabaabcbc".
- s = "dabaabcbc", remove "abc" starting at index 4, so s = "dababc".
- s = "dababc", remove "abc" starting at index 3, so s = "dab".
Now s has no occurrences of "abc".
```

**Example 2**

```
Input: s = "axxxxyyyyb", part = "xy"
Output: "ab"
Explanation: The following operations are done:
- s = "axxxxyyyyb", remove "xy" starting at index 4 so s = "axxxyyyb".
- s = "axxxyyyb", remove "xy" starting at index 3 so s = "axxyyb".
- s = "axxyyb", remove "xy" starting at index 2 so s = "axyb".
- s = "axyb", remove "xy" starting at index 1 so s = "ab".
Now s has no occurrences of "xy".
```

## Method 1

```tex
【O(n * m) time | O(n) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2025/02/11
 */
public class RemoveAllOccurrencesOfASubstring {

    public static String removeOccurrences(String s, String part) {
        // Convert input string to StringBuilder for efficient modification
        StringBuilder sb = new StringBuilder(s);
        // Store the length of the substring to be removed
        int partLen = part.length();

        // Variable to store the index of found substring
        int index;
        // Continue loop until no more occurrences of 'part' are found
        // indexOf() returns -1 when the substring is not found
        while ((index = sb.indexOf(part)) != -1) {
            // Remove the substring from StringBuilder
            // delete() removes characters from index to (index + partLen - 1)
            sb.delete(index, index + partLen);
        }

        // Convert StringBuilder back to String and return
        return sb.toString();
    }


    public static void main(String[] args) {
        // Test Case 1: Multiple overlapping removals
        String s1 = "daabcbaabcbc";
        String part1 = "abc";
        System.out.println("Test Case 1:");
        System.out.println("Input: s = " + s1 + ", part = " + part1);
        System.out.println("Output: " + removeOccurrences(s1, part1)); // Expected: "dab"

        // Test Case 2: Multiple consecutive removals
        String s2 = "axxxxyyyyb";
        String part2 = "xy";
        System.out.println("\nTest Case 2:");
        System.out.println("Input: s = " + s2 + ", part = " + part2);
        System.out.println("Output: " + removeOccurrences(s2, part2)); // Expected: "ab"

        // Test Case 3: Single removal at the end
        String s3 = "welcome";
        String part3 = "come";
        System.out.println("\nTest Case 3:");
        System.out.println("Input: s = " + s3 + ", part = " + part3);
        System.out.println("Output: " + removeOccurrences(s3, part3)); // Expected: "wel"
    }
}

```





