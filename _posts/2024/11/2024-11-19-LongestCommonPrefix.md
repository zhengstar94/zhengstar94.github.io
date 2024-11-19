---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "14. Longest Common Prefix"
date: "2024-11-19"
categories:
  - "LeetCode String"
---

- Write a function to find the longest common prefix string amongst an array of strings.
- If there is no common prefix, return an empty string `""`.

**Example 1**

```
Input: strs = [ "flower","flow","flight" ]
Output: "fl"
```

**Example 2**

```
Input: strs = [ "dog","racecar","car" ]
Output: ""
Explanation: There is no common prefix among the input strings.
```

## Method 1

```tex
【O(S) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2024/11/19
 */
public class LongestCommonPrefix {
    public static String longestCommonPrefix(String[] strs) {
        // Handle edge cases: return empty string if array is null or empty
        if(strs == null || strs.length == 0) {
            return "";
        }

        // If array contains only one string, return that string as the prefix
        if(strs.length == 1) {
            return strs[0];
        }

        // Initialize prefix with the first string
        String prefix = strs[0];

        // Iterate through the remaining strings in the array
        for (int i = 1; i < strs.length && prefix.length() > 0; i++) {
            /**
             * The indexOf(prefix) method returns the position where the prefix starts in the string
             * If prefix is found at the beginning of the string, indexOf returns 0
             * If prefix is not found at the beginning:
             * - Either prefix is not in the string at all (returns -1)
             * - Or prefix is found but not at the start (returns > 0)
             * In both cases, we need to shorten the prefix
             */
            while (strs[i].indexOf(prefix) != 0) {
                /**
                 * Shorten the prefix by removing its last character:
                 * - substring(0, prefix.length() - 1) creates a new string
                 * - Starting from index 0 (inclusive)
                 * - Ending at prefix.length() - 1 (exclusive)
                 * Example:
                 * If prefix = "flower", new prefix will be "flowe"
                 * This process continues until either:
                 * 1. The shortened prefix is found at the start of current string
                 * 2. Or prefix becomes empty string
                 */
                prefix = prefix.substring(0, prefix.length() - 1);

                // If prefix becomes empty, there is no common prefix
                if(prefix.isEmpty()) {
                    return "";
                }
            }
        }

        // Return the final common prefix found
        return prefix;
    }

    public static void main(String[] args) {
        // Test case 1: strings with common prefix "fl"
        String[] test1 = { "flower", "flow", "flight" };
        System.out.println("Test case 1: " + longestCommonPrefix(test1));  // Expected output: "fl"

        // Test case 2: strings with no common prefix
        String[] test2 = { "dog", "racecar", "car" };
        System.out.println("Test case 2: " + longestCommonPrefix(test2));  // Expected output: ""

        // Test case 3: empty array
        String[] test3 = {};
        System.out.println("Test case 3: " + longestCommonPrefix(test3));  // Expected output: ""

        // Test case 4: array with single string
        String[] test4 = { "single" };
        System.out.println("Test case 4: " + longestCommonPrefix(test4));  // Expected output: "single"
    }
}

```





