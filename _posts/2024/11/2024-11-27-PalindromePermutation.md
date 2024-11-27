---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "266. Palindrome Permutation"
date: "2024-11-27"
categories:
  - "LeetCode HashTable"
---


- Given a string, determine if a permutation of the string could form a palindrome.

**Example 1**

```
Input: "code"
Output: false
```

**Example 2**

```
Input: "aab"
Output: true
```

**Example 3**

```
Input: "carerac"
Output: true
```

## Method 1

```tex
【O(n) time | O(k) space】
```

```java
package Leetcode.HashTable;

import java.util.HashSet;
import java.util.Set;

/**
 * @author zhengxingxing
 * @date 2024/11/27
 */
public class PalindromePermutation {
    public static boolean canPermutePalindrome(String s) {
        // Create a HashSet to track character occurrences
        Set<Character> set = new HashSet<>();

        // Iterate through each character in the string
        for (char c : s.toCharArray()) {
            // If the character is already in the set, remove it
            // If not, add it to the set
            if(!set.add(c)){
                set.remove(c);
            }
        }

        // A palindrome can have at most one character with odd occurrence
        // So the set size should be 0 or 1
        return set.size() <= 1;
    }

    public static void main(String[] args) {
        // Test case 1: String that cannot form a palindrome
        String test1 = "code";
        System.out.println("Can form palindrome permutation: " + canPermutePalindrome(test1));

        // Test case 2: String that can form a palindrome (odd length)
        String test2 = "aab";
        System.out.println("Can form palindrome permutation: " + canPermutePalindrome(test2));

        // Test case 3: String that can form a palindrome (even length)
        String test3 = "carerac";
        System.out.println("Can form palindrome permutation: " + canPermutePalindrome(test3));

        // Test case 4: Empty string
        String test4 = "";
        System.out.println("Can form palindrome permutation: " + canPermutePalindrome(test4));

        // Test case 5: Single character
        String test5 = "a";
        System.out.println("Can form palindrome permutation: " + canPermutePalindrome(test5));
    }
}
```





