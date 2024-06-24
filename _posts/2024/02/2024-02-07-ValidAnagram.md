---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "242.Valid Anagram"
date: "2024-02-07"
categories:
  - "LeetCode Array"
---

# 242. Valid Anagram [Easy]

- Given two strings `s` and `t`, return `true` *if* `t` *is an anagram of* `s`*, and* `false` *otherwise*.
- An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1**

```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2**

```
Input: s = "rat", t = "car"
Output: false
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.Array;

/**
 * @author zhengstars
 * @date 2024/03/06
 */
public class ValidAnagram {
    public static boolean isAnagram(String s, String t) {
        // An array to hold the number of occurrences of each letter
        int [] arr = new int[26];
        // Convert the strings to character arrays
        char[] s1 = s.toCharArray();
        char[] t1 = t.toCharArray();
        // Iterate through each character in s
        for(char c : s1) {
            // Increment the counter for this character in the array
            arr[c-'a'] ++;
        }
        // Do the same for t
        // but decrement the counter
        for(char c : t1) {
            // Decrement the counter for this character in the array
            arr[c-'a'] --;
        }
        // Finally, check if all counts are 0
        for(int a : arr) {
            // If a count is not 0, the two strings are not anagrams of each other
            if(a != 0) {
                return false;
            }
        }
        // If all counts are 0, the strings are anagrams
        return true;
    }

    public static void main(String[] args) {

        // Begin testing
        System.out.println(isAnagram("anagram", "nagaram")); // should return true
        System.out.println(isAnagram("rat", "car")); // should return false
        System.out.println(isAnagram("aabbcc", "ccbbaa")); // should return true
        System.out.println(isAnagram("abcd", "dcbae")); // should return false
    }
}

```

