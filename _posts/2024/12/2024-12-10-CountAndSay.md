---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "38. Count and Say"
date: "2024-12-10"
tags: Medium
categories:
  - "LeetCode String"
---

- The **count-and-say** sequence is a sequence of digit strings defined by the recursive formula:

  - `countAndSay(1) = "1"`
  - `countAndSay(n)` is the run-length encoding of `countAndSay(n - 1)`.

- [Run-length encoding](http://en.wikipedia.org/wiki/Run-length_encoding) (RLE) is a string compression method that works by replacing consecutive identical characters (repeated 2 or more times) with the concatenation of the character and the number marking the count of the characters (length of the run). For example, to compress the string `"3322251"` we replace `"33"` with `"23"`, replace `"222"` with `"32"`, replace `"5"` with `"15"` and replace `"1"` with `"11"`. Thus the compressed string becomes `"23321511"`.

  Given a positive integer `n`, return *the* `nth` *element of the **count-and-say** sequence*.

**Example 1**

```
Input: n = 4
Output: "1211"

Explanation:
countAndSay(1) = "1"
countAndSay(2) = RLE of "1" = "11"
countAndSay(3) = RLE of "11" = "21"
countAndSay(4) = RLE of "21" = "1211"
```

**Example 2**

```
Input: n = 1
Output: "1"

Explanation:
This is the base case.
```

## Method 1

```tex
【O(2^n) time | O(2^n) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2024/12/10
 */
public class CountAndSay {
    public static String countAndSay(int n) {
        // Base case: when n = 1, return "1"
        if (n == 1) {
            return "1";
        }

        // Recursively get the previous sequence
        String prev = countAndSay(n - 1);

        // StringBuilder to build the current sequence
        StringBuilder result = new StringBuilder();

        // Variables to track the current character and its count
        char currentChar = prev.charAt(0);
        int count = 1;

        // Iterate through the previous sequence
        for (int i = 1; i < prev.length(); i++) {
            if (prev.charAt(i) == currentChar) {
                // Increment the count if the same character is repeated
                count++;
            } else {
                // Append the count and character to the result
                result.append(count).append(currentChar);

                // Reset the current character and its count
                currentChar = prev.charAt(i);
                count = 1;
            }
        }

        // Append the last group of characters
        result.append(count).append(currentChar);

        return result.toString();
    }

    public static void main(String[] args) {
        // Test case 1: n = 1
        int n1 = 1;
        System.out.println("Test case n = " + n1 + ": " + countAndSay(n1));

        // Test case 2: n = 2
        int n2 = 2;
        System.out.println("Test case n = " + n2 + ": " + countAndSay(n2));

        // Test case 3: n = 3
        int n3 = 3;
        System.out.println("Test case n = " + n3 + ": " + countAndSay(n3));

        // Test case 4: n = 4
        int n4 = 4;
        System.out.println("Test case n = " + n4 + ": " + countAndSay(n4));

        // Test case 5: n = 5
        int n5 = 5;
        System.out.println("Test case n = " + n5 + ": " + countAndSay(n5));
    }
}

```





