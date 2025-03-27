---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2825. Make String a Subsequence Using Cyclic Increments"
date: "2025-03-27"
tags: Medium TwoPointers
categories:
  - "LeetCode DoubleSeqSubsequencePointers"
---


- You are given two **0-indexed** strings `str1` and `str2`.
- In an operation, you select a **set** of indices in `str1`, and for each index `i` in the set, increment `str1[i]` to the next character **cyclically**. That is `'a'` becomes `'b'`, `'b'` becomes `'c'`, and so on, and `'z'` becomes `'a'`.
- Return `true` *if it is possible to make* `str2` *a subsequence of* `str1` *by performing the operation **at most once***, *and* `false` *otherwise*.
- **Note:** A subsequence of a string is a new string that is formed from the original string by deleting some (possibly none) of the characters without disturbing the relative positions of the remaining characters.

**Example 1**

```
Input: str1 = "abc", str2 = "ad"
Output: true
Explanation: Select index 2 in str1.
Increment str1[2] to become 'd'. 
Hence, str1 becomes "abd" and str2 is now a subsequence. Therefore, true is returned.
```

**Example 2**

```
Input: str1 = "zc", str2 = "ad"
Output: true
Explanation: Select indices 0 and 1 in str1. 
Increment str1[0] to become 'a'. 
Increment str1[1] to become 'd'. 
Hence, str1 becomes "ad" and str2 is now a subsequence. Therefore, true is returned.
```

**Example 3**

```
Input: str1 = "ab", str2 = "d"
Output: false
Explanation: In this example, it can be shown that it is impossible to make str2 a subsequence of str1 using the operation at most once. 
Therefore, false is returned.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqSubsequencePointers;

/**
 * @author zhengxingxing
 * @date 2025/03/27
 */
public class MakeStringASubsequenceUsingCyclicIncrements {
    
    public static boolean canMakeSubsequence(String str1, String str2) {
        // Initialize two pointers: i for str1 and j for str2
        int i = 0;
        int j = 0;

        // Continue while we haven't reached the end of either string
        while (i < str1.length() && j < str2.length()) {
            // Calculate the next character after cyclic increment
            // For example: 'a' -> 'b', 'b' -> 'c', ..., 'z' -> 'a'
            // Formula explanation:
            // 1. str1.charAt(i) - 'a': Convert char to 0-based index (0-25)
            // 2. + 1: Increment to next character
            // 3. % 26: Handle cyclic wrap-around ('z' -> 'a')
            // 4. + 'a': Convert back to character
            char next = (char)((str1.charAt(i) - 'a' + 1) % 26 + 'a');

            // If current character in str1 matches str2
            // OR the next character after increment matches str2
            // then advance the j pointer
            if(str1.charAt(i) == str2.charAt(j) || next == str2.charAt(j)) {
                j++;
            }
            // Always advance the i pointer to check next character in str1
            i++;
        }

        // Return true if we matched all characters in str2 (j reached the end)
        return j == str2.length();
    }

    public static void main(String[] args) {
        // Test Case 1: Should return true
        // We can change 'c' to 'd' to make "abd", which contains "ad"
        String str1_1 = "abc";
        String str2_1 = "ad";
        System.out.println("Test Case 1 Result: " + canMakeSubsequence(str1_1, str2_1));

        // Test Case 2: Should return true
        // We can change 'z' to 'a' and 'c' to 'd' to make "ad"
        String str1_2 = "zc";
        String str2_2 = "ad";
        System.out.println("Test Case 2 Result: " + canMakeSubsequence(str1_2, str2_2));

        // Test Case 3: Should return false
        // Cannot make "d" a subsequence with just one increment operation
        String str1_3 = "ab";
        String str2_3 = "d";
        System.out.println("Test Case 3 Result: " + canMakeSubsequence(str1_3, str2_3));
    }
}

```





