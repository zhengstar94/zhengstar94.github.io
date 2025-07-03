---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3304. Find the K-th Character in String Game I"
date: "2025-07-03"
tags: Easy
categories:
  - "LeetCode Math"
---


- Alice and Bob are playing a game. Initially, Alice has a string `word = "a"`.
- You are given a **positive** integer `k`.
- Now Bob will ask Alice to perform the following operation **forever**:
  - Generate a new string by **changing** each character in `word` to its **next** character in the English alphabet, and **append** it to the *original* `word`.
- For example, performing the operation on `"c"` generates `"cd"` and performing the operation on `"zb"` generates `"zbac"`.
- Return the value of the `kth` character in `word`, after enough operations have been done for `word` to have **at least** `k` characters.
- **Note** that the character `'z'` can be changed to `'a'` in the operation.

**Example 1**

```
Input: k = 5

Output: "b"

Explanation:

Initially, word = "a". We need to do the operation three times:

Generated string is "b", word becomes "ab".
Generated string is "bc", word becomes "abbc".
Generated string is "bccd", word becomes "abbcbccd".
```

**Example 2**

```
Input: k = 10

Output: "c"
```

## Method 1

```tex
【O(log k) time | O(log k) space】
```

```java
package Leetcode.Math;

/**
 * @Author zhengxingxing
 * @Date 2025/07/03
 */
public class FindTheKthCharacterInStringGameI {


    public static char kthCharacter(int k) {
        // Start the recursive search with offset 0 (no character has been shifted yet)
        return helper(k, 0);
    }

    private static char helper(int k, int offset) {
        // Base case: if k == 1, we are at the very first character of the current segment.
        // The character is 'a' shifted by 'offset' positions in the alphabet (with wrap-around).
        if (k == 1) {
            return (char)('a' + offset % 26);
        }

        // Find the largest power of 2 (len) such that len * 2 < k.
        // This represents the length of the previous string before the current operation doubled it.
        int len = 1;
        while (len * 2 < k) {
            len *= 2;
        }

        // If k is within the first half (<= len), it means the character is from the previous string segment,
        // and has not been shifted in this operation. So, we recurse with the same offset.
        if (k <= len) {
            return helper(k, offset);
        } else {
            // If k is in the second half (> len), it corresponds to a character from the first half,
            // but shifted by one (i.e., "next letter" operation). So, we recurse for (k - len) and increment offset.
            return helper(k - len, offset + 1);
        }
    }

    public static void main(String[] args) {
        System.out.println(kthCharacter(5));   // Output: b
        System.out.println(kthCharacter(10));  // Output: c
    }
}

```





