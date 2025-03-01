---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "131. Palindrome Partitioning"
date: "2025-03-01"
tags: Medium
categories:
  - "LeetCode Backtracking"
---

- Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return *all possible palindrome partitioning of* `s`.

**Example 1**

```
Input: s = "aab"
Output: [ [ "a","a","b"],["aa","b" ] ]
```

**Example 2**

```
Input: s = "a"
Output: [ [ "a" ] ]
```

## Method 1

```tex
【O(n * 2^n) time | O(n) space】
```

```java
package Leetcode.Backtracking;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/03/01
 */
public class PalindromePartitioning {
    
    public static List<List<String>> partition(String s) {
        List<List<String>> result = new ArrayList<>();  // Store all valid partitioning results
        backtrack(result, new ArrayList<>(), s, 0);     // Start backtracking from index 0
        return result;
    }

    /**
     * Backtracking method to find all possible palindrome partitions
     *
     * @param result   List to store all valid partitioning results
     * @param tempList Current list storing the palindrome substrings in this path
     * @param s        Original input string
     * @param start    Current starting position for partitioning
     *
     * Example for string "aaba":
     * 1st recursion (start=0): considers "a", "aa", "aab", "aaba"
     * 2nd recursion (start=1): considers "a", "ab", "aba"
     * 3rd recursion (start=2): considers "b", "ba"
     * 4th recursion (start=3): considers "a"
     */
    private static void backtrack(List<List<String>> result, List<String> tempList, String s, int start) {
        // Base case: if we've reached the end of the string
        // This means we've found a valid partition (all substrings are palindromes)
        if (start >= s.length()) {
            result.add(new ArrayList<>(tempList));  // Add a deep copy of current partition to result
            return;
        }

        // Try all possible substrings starting from 'start' position
        for (int i = start; i < s.length(); i++) {
            // Check if substring from start to i is palindrome
            // Example: for "aaba", when start=0, i=1
            // First checks "a", then "aa", then "aab", then "aaba"
            if (isPalindrome(s, start, i)) {
                // Add current palindrome substring to tempList
                // Example: if "aa" is palindrome, tempList = ["aa"]
                tempList.add(s.substring(start, i + 1));

                // Recursively process the rest of the string
                // Example: after adding "aa", next recursion starts at position 2
                // This means we'll now look for palindromes in "ba"
                backtrack(result, tempList, s, i + 1);

                // Backtrack: remove the last added substring to try other possibilities
                // Example: after trying "aa" + "b" + "a", remove "a" to try other combinations
                // This is crucial for exploring all possible partitions
                tempList.remove(tempList.size() - 1);
            }
        }
    }

    
    private static boolean isPalindrome(String s, int start, int end) {
        while (start < end) {
            if (s.charAt(start) != s.charAt(end)) {
                return false;
            }
            start++;
            end--;
        }
        return true;
    }

    
    public static void main(String[] args) {
        // Test case 1: "aab" -> [ [ "a","a","b"], ["aa","b" ] ]
        String s1 = "aab";
        System.out.println("Test case 1 input: " + s1);
        System.out.println("Test case 1 output: " + partition(s1));

        // Test case 2: "a" -> [ [ "a" ] ]
        String s2 = "a";
        System.out.println("Test case 2 input: " + s2);
        System.out.println("Test case 2 output: " + partition(s2));
    }
}

```





