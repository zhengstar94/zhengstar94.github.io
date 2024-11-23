---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "791. Custom Sort String"
date: "2024-11-23"
categories:
  - "LeetCode HashTable"
---

- You are given two strings `order` and `s`. All the characters of `order` are **unique** and were sorted in some custom order previously.
- Permute the characters of `s` so that they match the order that `order` was sorted. More specifically, if a character `x` occurs before a character `y` in `order`, then `x` should occur before `y` in the permuted string.
- Return *any permutation of* `s` *that satisfies this property*.

**Example 1**

```
Input: order = "cba", s = "abcd"

Output: "cbad"

Explanation: "a", "b", "c" appear in order, so the order of "a", "b", "c" should be "c", "b", and "a".

Since "d" does not appear in order, it can be at any position in the returned string. "dcba", "cdba", "cbda" are also valid outputs.
```

**Example 2**

```
Input: order = "bcafg", s = "abcd"

Output: "bcad"

Explanation: The characters "b", "c", and "a" from order dictate  the order for the characters in s. The character "d" in s does not appear in order, so its position is flexible.

Following the order of appearance in order, "b", "c", and "a" from s should be arranged as "b", "c", "a". "d" can be placed at any position since it's not in order. The output "bcad" correctly follows this rule. Other arrangements like "dbca" or "bcda" would also be valid, as long as "b", "c", "a" maintain their order.
```

## Method 1

```tex
【O(n + m) time | O(n) space】
```

```java
package Leetcode.HashTable;

/**
 * @author zhengxingxing
 * @date 2024/11/23
 */
public class CustomSortString {
    public static String customSortString(String order, String s) {
        // Create a frequency array to store count of each character in s
        // Index 0 represents 'a', 1 represents 'b', and so on
        int[] count = new int[26];

        // Count frequency of each character in string s
        for (char c : s.toCharArray()) {
            count[c - 'a']++;
        }

        // StringBuilder to construct the result efficiently
        StringBuilder result = new StringBuilder();

        // First, append characters according to the order string
        // For each character in order, append it to result as many times as it appears in s
        for (char c: order.toCharArray()) {
            while (count[c - 'a'] > 0 ){
                result.append(c);
                // Decrement count after appending each character
                count[c - 'a']--;
            }
        }

        // Finally, append any remaining characters that weren't in order
        // This ensures all characters from s are included in the result
        for (char c = 'a'; c <= 'z' ; c++) {
            while (count[c - 'a'] > 0 ){
                result.append(c);
                count[c - 'a']--;
            }
        }

        // Convert StringBuilder to String and return
        return result.toString();
    }

    public static void main(String[] args) {
        // Test Case 1: Basic test
        String order1 = "cba";
        String s1 = "abcd";
        System.out.println("Test Case 1:");
        System.out.println("Input: order = " + order1 + ", s = " + s1);
        System.out.println("Output: " + customSortString(order1, s1));
        System.out.println();

        // Test Case 2: Order string longer than s
        String order2 = "bcafg";
        String s2 = "abcd";
        System.out.println("Test Case 2:");
        System.out.println("Input: order = " + order2 + ", s = " + s2);
        System.out.println("Output: " + customSortString(order2, s2));
        System.out.println();

        // Test Case 3: String s contains duplicate characters
        String order3 = "cba";
        String s3 = "aabcd";
        System.out.println("Test Case 3 (contains duplicate characters):");
        System.out.println("Input: order = " + order3 + ", s = " + s3);
        System.out.println("Output: " + customSortString(order3, s3));
        System.out.println();

        // Test Case 4: Order contains characters not in s
        String order4 = "cbafg";
        String s4 = "abc";
        System.out.println("Test Case 4 (order contains characters not in s):");
        System.out.println("Input: order = " + order4 + ", s = " + s4);
        System.out.println("Output: " + customSortString(order4, s4));
        System.out.println();

        // Test Case 5: String s contains characters not in order
        String order5 = "cb";
        String s5 = "abcd";
        System.out.println("Test Case 5 (s contains characters not in order):");
        System.out.println("Input: order = " + order5 + ", s = " + s5);
        System.out.println("Output: " + customSortString(order5, s5));
    }
}
```

