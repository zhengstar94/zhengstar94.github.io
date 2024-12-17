---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2182. Construct String With Repeat Limit"
date: "2024-12-17"
tags: Medium
categories:
  - "LeetCode Greedy"
---

- You are given a string `s` and an integer `repeatLimit`. Construct (vt. 修建, 建立 构成, 组成) a new string `repeatLimitedString` using the characters of `s` such that no letter appears **more than** `repeatLimit` times **in a row**. You do **not** have to use all characters from `s`.
- Return *the **lexicographically largest*** `repeatLimitedString` *possible*.
- A string `a` is **lexicographically larger** than a string `b` if in the first position where `a` and `b` differ, string `a` has a letter that appears later in the alphabet than the corresponding letter in `b`. If the first `min(a.length, b.length)` characters do not differ, then the longer string is the lexicographically larger one.


**Example 1**

```
Input: s = "cczazcc", repeatLimit = 3
Output: "zzcccac"
Explanation: We use all of the characters from s to construct the repeatLimitedString "zzcccac".
The letter 'a' appears at most 1 time in a row.
The letter 'c' appears at most 3 times in a row.
The letter 'z' appears at most 2 times in a row.
Hence, no letter appears more than repeatLimit times in a row and the string is a valid repeatLimitedString.
The string is the lexicographically largest repeatLimitedString possible so we return "zzcccac".
Note that the string "zzcccca" is lexicographically larger but the letter 'c' appears more than 3 times in a row, so it is not a valid repeatLimitedString.
```

**Example 2**

```
Input: s = "aababab", repeatLimit = 2
Output: "bbabaa"
Explanation: We use only some of the characters from s to construct the repeatLimitedString "bbabaa". 
The letter 'a' appears at most 2 times in a row.
The letter 'b' appears at most 2 times in a row.
Hence, no letter appears more than repeatLimit times in a row and the string is a valid repeatLimitedString.
The string is the lexicographically largest repeatLimitedString possible so we return "bbabaa".
Note that the string "bbabaaa" is lexicographically larger but the letter 'a' appears more than 2 times in a row, so it is not a valid repeatLimitedString.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Greedy;

/**
 * @author zhengxingxing
 * @date 2024/12/17
 */
public class ConstructStringWithRepeatLimit {
    public static String repeatLimitedString(String s, int repeatLimit) {
        int[] count = new int[26];
        // Count the occurrences of each character in the string
        for (char ch : s.toCharArray()) {
            count[ch - 'a']++;
        }

        // Start from the largest character
        int r = 25;
        StringBuilder sb = new StringBuilder();

        for (int i = 25; i >= 0; i--) {
            // Ensure that r does not exceed the previous smaller character
            r = Math.min(r, i - 1);

            while (count[i] != 0) {
                if (count[i] > repeatLimit) {
                    // Add `repeatLimit` occurrences of the current character
                    int curr = repeatLimit;
                    while (curr-- > 0) {
                        sb.append((char) ('a' + i));
                    }

                    // Find the next available smaller character
                    while (r >= 0 && count[r] == 0) {
                        r--;
                    }

                    // If no smaller character is available, return the constructed string
                    if (r < 0) {
                        return sb.toString();
                    }

                    // Add the smaller character found
                    sb.append((char) ('a' + r));
                    count[r]--;
                    count[i] -= repeatLimit;
                } else {
                    // Add all remaining occurrences of the current character
                    int curr = count[i];
                    while (curr-- > 0) {
                        sb.append((char) ('a' + i));
                    }
                    count[i] = 0;
                }
            }
        }

        return sb.toString();
    }

    public static void main(String[] args) {
        // Test Case 1: Basic scenario
        String s1 = "aabccc";
        int repeatLimit1 = 2;
        String result1 = repeatLimitedString(s1, repeatLimit1);
        System.out.println("Test Case 1:");
        System.out.println("Input String: " + s1);
        System.out.println("Repeat Limit: " + repeatLimit1);
        System.out.println("Result: " + result1);
        System.out.println();

        // Test Case 2: All characters are the same
        String s2 = "aaaaa";
        int repeatLimit2 = 2;
        String result2 = repeatLimitedString(s2, repeatLimit2);
        System.out.println("Test Case 2:");
        System.out.println("Input String: " + s2);
        System.out.println("Repeat Limit: " + repeatLimit2);
        System.out.println("Result: " + result2);
        System.out.println();

        // Test Case 3: Complex scenario
        String s3 = "cczzzzeeddaabb";
        int repeatLimit3 = 3;
        String result3 = repeatLimitedString(s3, repeatLimit3);
        System.out.println("Test Case 3:");
        System.out.println("Input String: " + s3);
        System.out.println("Repeat Limit: " + repeatLimit3);
        System.out.println("Result: " + result3);
        System.out.println();

        // Test Case 4: Repeat limit is 1
        String s4 = "aabbccddee";
        int repeatLimit4 = 1;
        String result4 = repeatLimitedString(s4, repeatLimit4);
        System.out.println("Test Case 4:");
        System.out.println("Input String: " + s4);
        System.out.println("Repeat Limit: " + repeatLimit4);
        System.out.println("Result: " + result4);
    }
}

```





