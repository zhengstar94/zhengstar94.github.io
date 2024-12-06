---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "434. Number of Segments in a String"
date: "2024-12-06"
tags: Easy
categories:
  - "LeetCode String"
---

- Given a string `s`, return *the number of segments in the string*.
- A **segment** is defined to be a contiguous sequence of **non-space characters**.

**Example 1**

```
Input: s = "Hello, my name is John"
Output: 5
Explanation: The five segments are ["Hello,", "my", "name", "is", "John"]
```

**Example 2**

```
Input: s = "Hello"
Output: 1
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2024/12/06
 */
public class NumberOfSegmentsInAString {
    public static int countSegments(String s) {
        int sum = 0; // Initialize the segment counter.

        // Loop through the characters in the string.
        for (int i = 0; i < s.length(); i++) {
            /**
             * Key condition:
             * 1. Check if the current character is NOT a space: s.charAt(i) != ' '
             *    - Ensures we are only processing non-space characters.
             * 2. Check if it's the start of a new segment:
             *    - It's the first character in the string (i == 0), OR
             *    - The previous character is a space (s.charAt(i - 1) == ' ').
             *    - This ensures that we only count the beginning of a new word.
             */
            if (s.charAt(i) != ' ' && (i == 0 || s.charAt(i - 1) == ' ')) {
                sum++; // Increment the segment counter for a new word.
            }
        }

        return sum; // Return the total number of segments found.
    }

    public static void main(String[] args) {
        // Test cases
        String test1 = "Hello world"; // Two words.
        String test2 = "   Leading spaces"; // Two words with leading spaces.
        String test3 = "Trailing spaces   "; // Two words with trailing spaces.
        String test4 = "  Multiple   spaces  between words  "; // Four words with multiple spaces.
        String test5 = ""; // Empty string, should return 0.
        String test6 = "      "; // String with only spaces, should return 0.
        String test7 = "OneWord"; // Single word, should return 1.

        // Print results for each test case.
        System.out.println("Test1: " + test1 + " -> Segments: " + countSegments(test1)); // Output: 2
        System.out.println("Test2: " + test2 + " -> Segments: " + countSegments(test2)); // Output: 2
        System.out.println("Test3: " + test3 + " -> Segments: " + countSegments(test3)); // Output: 2
        System.out.println("Test4: " + test4 + " -> Segments: " + countSegments(test4)); // Output: 4
        System.out.println("Test5: " + test5 + " -> Segments: " + countSegments(test5)); // Output: 0
        System.out.println("Test6: " + test6 + " -> Segments: " + countSegments(test6)); // Output: 0
        System.out.println("Test7: " + test7 + " -> Segments: " + countSegments(test7)); // Output: 1
    }
}


```





