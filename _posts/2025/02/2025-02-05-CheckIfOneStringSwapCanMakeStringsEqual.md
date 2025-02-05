---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1790. Check if One String Swap Can Make Strings Equal"
date: "2025-02-05"
tags: Easy
categories:
  - "LeetCode String"
---


- You are given two strings `s1` and `s2` of equal length. A **string swap** is an operation where you choose two indices in a string (not necessarily different) and swap the characters at these indices.
- Return `true` *if it is possible to make both strings equal by performing **at most one string swap** on **exactly one** of the strings.* Otherwise, return `false`.

**Example 1**

```
Input: s1 = "bank", s2 = "kanb"
Output: true
Explanation: For example, swap the first character with the last character of s2 to make "bank".
```

**Example 2**

```
Input: s1 = "attack", s2 = "defend"
Output: false
Explanation: It is impossible to make them equal with one string swap.
```

**Example 3**

```
Input: s1 = "kelb", s2 = "kelb"
Output: true
Explanation: The two strings are already equal, so no string swap operation is required.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2025/02/05
 */
public class CheckIfOneStringSwapCanMakeStringsEqual {
    public static boolean areAlmostEqual(String s1, String s2) {
        // If the lengths of the two strings are not equal, return false
        if (s1.length() != s2.length()) {
            return false;
        }

        // Variables to store indices of different characters
        // i1 will store the index of the first differing character
        // i2 will store the index of the second differing character
        int i1 = -1;
        int i2 = -1;

        // Iterate through each character of both strings
        for (int i = 0; i < s1.length(); i++) {
            // If characters at the current index are the same, continue to the next index
            if (s1.charAt(i) == s2.charAt(i)) {
                continue;
            }

            // If we already found two differences and another one is found,
            // then we need more than one swap, hence return false
            if (i2 != -1) {
                return false;
            }

            // If this is the first differing character, store its index
            if (i1 == -1) {
                i1 = i;
            } else {
                // This is the second differing character, store its index
                i2 = i;
            }
        }

        // If no differing characters were found, it means both strings are already equal
        if (i1 == -1 && i2 == -1) {
            return true;
        }

        // If we found only one differing character, return false
        // Because a single swap can only fix two differing characters
        if (i2 == -1) {
            return false;
        }

        // Finally, check if swapping the characters at these two indices
        // would make the two strings equal
        return s1.charAt(i1) == s2.charAt(i2) && s1.charAt(i2) == s2.charAt(i1);
    }

    public static void main(String[] args) {
        // Test case 1
        String s1 = "bank";
        String s2 = "kanb";
        System.out.println("Test Case 1: " + areAlmostEqual(s1, s2)); // Expected output: true

        // Test case 2
        s1 = "attack";
        s2 = "defend";
        System.out.println("Test Case 2: " + areAlmostEqual(s1, s2)); // Expected output: false

        // Test case 3
        s1 = "kelb";
        s2 = "kelb";
        System.out.println("Test Case 3: " + areAlmostEqual(s1, s2)); // Expected output: true

        // Test case 4 (where there's a possible swap)
        s1 = "abcd";
        s2 = "badc";
        System.out.println("Test Case 4: " + areAlmostEqual(s1, s2)); // Expected output: false

        // Test case 5 (where too many differences)
        s1 = "abc";
        s2 = "xyz";
        System.out.println("Test Case 5: " + areAlmostEqual(s1, s2)); // Expected output: false
    }
}

```





