---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2185. Counting Words With a Given Prefix"
date: "2025-01-09"
tags: Easy
categories:
  - "LeetCode String"
---


- You are given an array of strings `words` and a string `pref`.
- Return *the number of strings in* `words` *that contain* `pref` *as a **prefix***.
- A **prefix** of a string `s` is any leading contiguous substring of `s`.

**Example 1**

```
Input: words = ["pay","attention","practice","attend"], pref = "at"
Output: 2
Explanation: The 2 strings that contain "at" as a prefix are: "attention" and "attend".
```

**Example 2**

```
Input: words = ["leetcode","win","loops","success"], pref = "code"
Output: 0
Explanation: There are no strings that contain "code" as a prefix.
```

## Method 1

```tex
【O(n * m) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2025/01/09
 */
public class CountingWordsWithAGivenPrefix {
    public static int prefixCount(String[] words, String pref) {
        int count = 0;
        for (String word : words) {
            if (word.startsWith(pref)) {
                count++;
            }
        }
        return count;
    }

    public static void main(String[] args) {
        // Test case 1
        String[] words1 = {"pay", "attention", "practice", "attend"};
        String pref1 = "at";
        System.out.println("Test case 1 result: " + prefixCount(words1, pref1)); // Expected output: 2

        // Test case 2
        String[] words2 = {"leetcode", "win", "loops", "success"};
        String pref2 = "code";
        System.out.println("Test case 2 result: " + prefixCount(words2, pref2)); // Expected output: 0

        // Additional test case
        String[] words3 = {"apple", "app", "apartment", "ape", "banana"};
        String pref3 = "ap";
        System.out.println("Test case 3 result: " + prefixCount(words3, pref3)); // Expected output: 4
    }

}

```





