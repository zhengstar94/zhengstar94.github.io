---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2278. Percentage of Letter in String"
date: "2025-03-31"
tags: Easy
categories:
  - "LeetCode String"
---

- Given a string `s` and a character `letter`, return *the **percentage** of characters in* `s` *that equal* `letter` ***rounded down** to the nearest whole percent.*

**Example 1**

```
Input: s = "foobar", letter = "o"
Output: 33
Explanation:
The percentage of characters in s that equal the letter 'o' is 2 / 6 * 100% = 33% when rounded down, so we return 33.
```

**Example 2**

```
Input: s = "jjjj", letter = "k"
Output: 0
Explanation:
The percentage of characters in s that equal the letter 'k' is 0%, so we return 0.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2025/03/31
 */
public class PercentageOfLetterInString {
    
    public static int percentageLetter(String s, char letter) {
        // Initialize counter to track letter occurrences
        int count = 0;

        // Iterate through each character in the string
        for (char c : s.toCharArray()) {
            // Increment counter when matching letter is found
            if (c == letter) {
                count++;
            }
        }

        // Calculate and return the percentage rounded down
        return (count * 100) / s.length();
    }

    public static void main(String[] args) {
        // Test Case 1: String with multiple occurrences of target letter
        String s1 = "foobar";
        char letter1 = 'o';
        System.out.println("Test Case 1 Result: " + percentageLetter(s1, letter1)); // Expected output: 33

        // Test Case 2: String without any occurrence of target letter
        String s2 = "jjjj";
        char letter2 = 'k';
        System.out.println("Test Case 2 Result: " + percentageLetter(s2, letter2)); // Expected output: 0

        // Test Case 3: Additional test case with target letter 'e'
        String s3 = "percentage";
        char letter3 = 'e';
        System.out.println("Test Case 3 Result: " + percentageLetter(s3, letter3)); // Expected output: 30
    }
}

```





