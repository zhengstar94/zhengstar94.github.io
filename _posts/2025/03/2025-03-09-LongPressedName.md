---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "925. Long Pressed Name"
date: "2025-03-09"
tags: Easy TwoPointers
categories:
  - "LeetCode DoubleSeqTwoPointers"
---


- Your friend is typing his `name` into a keyboard. Sometimes, when typing a character `c`, the key might get *long pressed*, and the character will be typed 1 or more times.
- You examine the `typed` characters of the keyboard. Return `True` if it is possible that it was your friends name, with some characters (possibly none) being long pressed.

**Example 1**

```
Input: name = "alex", typed = "aaleex"
Output: true
Explanation: 'a' and 'e' in 'alex' were long pressed.
```

**Example 2**

```
Input: name = "saeed", typed = "ssaaedd"
Output: false
Explanation: 'e' must have been pressed twice, but it was not in the typed output.
```

## Method 1

```tex
【O(n + m) time | O(1) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqTwoPointers;

/**
 * @author zhengxingxing
 * @date 2025/03/09
 */
public class LongPressedName {
   
    public static boolean isLongPressedName(String name, String typed) {
        // Initialize two pointers: i for name string, j for typed string
        int i = 0;  // pointer for name string
        int j = 0;  // pointer for typed string

        while (j < typed.length()) {
            // Case 1: Characters match - advance both pointers
            if (i < name.length() && name.charAt(i) == typed.charAt(j)) {
                i++;
                j++;
            }
            // Case 2: Long-pressed character - check if current char matches previous char
            else if (j > 0 && typed.charAt(j) == typed.charAt(j - 1)) {
                j++;
            }
            // Case 3: Neither matches nor long-pressed - invalid input
            else {
                return false;
            }
        }

        // Final check: ensure we've matched all characters in name
        return i == name.length();
    }
    
    public static void main(String[] args) {
        // Test Case 1: Normal long-press scenario
        String name1 = "alex";
        String typed1 = "aaleex";
        System.out.println("Test Case 1 Result: " +
                isLongPressedName(name1, typed1));  // Expected output: true

        // Test Case 2: Invalid long-press scenario
        String name2 = "saeed";
        String typed2 = "ssaaedd";
        System.out.println("Test Case 2 Result: " +
                isLongPressedName(name2, typed2));  // Expected output: false

        // Test Case 3: Exact match scenario
        String name3 = "alex";
        String typed3 = "alex";
        System.out.println("Test Case 3 Result: " +
                isLongPressedName(name3, typed3));  // Expected output: true

        // Test Case 4: Typed string shorter than name
        String name4 = "alex";
        String typed4 = "ale";
        System.out.println("Test Case 4 Result: " +
                isLongPressedName(name4, typed4));  // Expected output: false

        // Test Case 5: Completely different strings
        String name5 = "alex";
        String typed5 = "bbbb";
        System.out.println("Test Case 5 Result: " +
                isLongPressedName(name5, typed5));  // Expected output: false
    }
}

```





