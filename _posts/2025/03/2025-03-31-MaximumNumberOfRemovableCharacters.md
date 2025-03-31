---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1898. Maximum Number of Removable Characters"
date: "2025-03-31"
tags: Medium TwoPointers
categories:
  - "LeetCode DoubleSeqSubsequencePointers"
---


- You are given two strings `s` and `p` where `p` is a **subsequence** of `s`. You are also given a **distinct 0-indexed** integer array `removable` containing a subset of indices of `s` (`s` is also **0-indexed**).
- You want to choose an integer `k` (`0 <= k <= removable.length`) such that, after removing `k` characters from `s` using the **first** `k` indices in `removable`, `p` is still a **subsequence** of `s`. More formally, you will mark the character at `s[removable[i]]` for each `0 <= i < k`, then remove all marked characters and check if `p` is still a subsequence.
- Return *the **maximum*** `k` *you can choose such that* `p` *is still a **subsequence** of* `s` *after the removals*.
- A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

**Example 1**

```
Input: s = "abcacb", p = "ab", removable = [3,1,0]
Output: 2
Explanation: After removing the characters at indices 3 and 1, "abcacb" becomes "accb".
"ab" is a subsequence of "accb".
If we remove the characters at indices 3, 1, and 0, "abcacb" becomes "ccb", and "ab" is no longer a subsequence.
Hence, the maximum k is 2.
```

**Example 2**

```
Input: s = "abcbddddd", p = "abcd", removable = [3,2,1,4,5,6]
Output: 1
Explanation: After removing the character at index 3, "abcbddddd" becomes "abcddddd".
"abcd" is a subsequence of "abcddddd".
```

**Example 3**

```
Input: s = "abcab", p = "abc", removable = [0,1,2,3,4]
Output: 0
Explanation: If you remove the first index in the array removable, "abc" is no longer a subsequence.
```

## Method 1

```tex
【O(n * log(m)) time | O(n) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqSubsequencePointers;

/**
 * @author zhengxingxing
 * @date 2025/03/31
 */
public class MaximumNumberOfRemovableCharacters {

    public static int maximumRemovals(String s, String p, int[] removable) {
        // Initialize binary search boundaries
        int left = 0;
        int right = removable.length;

        // Binary search process
        while (left < right) {
            // Calculate middle point (using ceiling division to avoid infinite loop)
            int mid = left + (right - left + 1) / 2;

            // If we can remove mid characters, try removing more
            if (canRemoveKCharacters(s, p, removable, mid)) {
                left = mid;
            } else {
                // If we can't remove mid characters, try removing fewer
                right = mid - 1;
            }
        }
        return left;
    }


    private static boolean canRemoveKCharacters(String s, String p, int[] removable, int k) {
        // Create boolean array to mark characters that should be removed
        boolean[] removed = new boolean[s.length()];

        // Mark first k positions from removable array as true
        for (int i = 0; i < k; i++) {
            removed[removable[i]] = true;
        }

        // Use two pointers to check if p is still a subsequence
        int j = 0;  // Pointer for string p
        for (int i = 0; i < s.length() && j < p.length(); i++) {
            // Skip removed characters
            if (removed[i]) {
                continue;
            }

            // If characters match, advance pointer for string p
            if (s.charAt(i) == p.charAt(j)) {
                j++;
            }
        }

        // Return true if we found all characters of p
        return j == p.length();
    }
    
    public static void main(String[] args) {
        // Test Case 1
        String s1 = "abcacb";
        String p1 = "ab";
        int[] removable1 = {3,1,0};
        System.out.println("Test Case 1 Result: " + maximumRemovals(s1, p1, removable1));

        // Test Case 2
        String s2 = "abcbddddd";
        String p2 = "abcd";
        int[] removable2 = {3,2,1,4,5,6};
        System.out.println("Test Case 2 Result: " + maximumRemovals(s2, p2, removable2));

        // Test Case 3
        String s3 = "abcab";
        String p3 = "abc";
        int[] removable3 = {0,1,2,3,4};
        System.out.println("Test Case 3 Result: " + maximumRemovals(s3, p3, removable3));
    }
}

```





