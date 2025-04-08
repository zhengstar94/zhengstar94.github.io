---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1446. Consecutive Characters"
date: "2025-04-08"
tags: Easy
categories:
  - "LeetCode GroupedLoop"
---


- The **power** of the string is the maximum length of a non-empty substring that contains only one unique character.
- Given a string `s`, return *the **power** of* `s`.

**Example 1**

```
Input: s = "leetcode"
Output: 2
Explanation: The substring "ee" is of length 2 with the character 'e' only.
```

**Example 2**

```
Input: s = "abbcccddddeeeeedcba"
Output: 5
Explanation: The substring "eeeee" is of length 5 with the character 'e' only.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/08
 */
public class ConsecutiveCharacters {
    
    public static int maxPower(String s) {
        // Get string length
        int n = s.length();
        // Track maximum consecutive count found
        int maxCount = 0;
        // Current position in string
        int i = 0;

        // Process string using group cycle pattern
        while (i < n) {
            // Mark start of current group
            int start = i;
            // Get character for current group
            char currentChar = s.charAt(start);

            // Extend group while same character continues
            while (i < n && s.charAt(i) == currentChar) {
                i++;
            }

            // Calculate length of current group
            int groupLength = i - start;
            // Update maximum count if current group is longer
            maxCount = Math.max(maxCount, groupLength);
        }

        return maxCount;
    }
    
    public static void main(String[] args) {
        // Test case 1: String with two consecutive 'e's
        System.out.println("Test case 1: " + maxPower("leetcode"));  // Expected: 2 (ee)

        // Test case 2: String with multiple groups of increasing length
        System.out.println("Test case 2: " + maxPower("abbcccddddeeeee"));  // Expected: 5 (eeeee)

        // Test case 3: Edge case - single character
        System.out.println("Test case 3: " + maxPower("a"));  // Expected: 1

        // Test case 4: Edge case - no consecutive characters
        System.out.println("Test case 4: " + maxPower("abc"));  // Expected: 1
    }
}

```





