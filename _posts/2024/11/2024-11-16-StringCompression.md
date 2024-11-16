---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "443. String Compression"
date: "2024-11-16"
categories:
  - "LeetCode Two Pointers"
---

- Given an array of characters `chars`, compress it using the following algorithm:
- Begin with an empty string `s`. For each group of **consecutive repeating characters** in `chars`:
  - If the group's length is `1`, append the character to `s`.
  - Otherwise, append the character followed by the group's length.
- The compressed string `s` **should not be returned separately**, but instead, be stored **in the input character array `chars`**. Note that group lengths that are `10` or longer will be split into multiple characters in `chars`.
- After you are done **modifying the input array,** return *the new length of the array*.
- You must write an algorithm that uses only constant extra space.

**Example 1**

```
Input: chars = [ "a","a","b","b","c","c","c" ]
Output: Return 6, and the first 6 characters of the input array should be: [ "a","2","b","2","c","3" ]
Explanation: The groups are "aa", "bb", and "ccc". This compresses to "a2b2c3".
```

**Example 2**

```
Input: chars = [ "a" ]
Output: Return 1, and the first character of the input array should be: [ "a" ]
Explanation: The only group is "a", which remains uncompressed since it's a single character.
```

**Example 3**

```
Input: chars = [ "a","b","b","b","b","b","b","b","b","b","b","b","b" ]
Output: Return 4, and the first 4 characters of the input array should be: [ "a","b","1","2" ].
Explanation: The groups are "a" and "bbbbbbbbbbbb". This compresses to "ab12".
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2024/11/16
 */
public class StringCompression {
    public static int compress(char[] chars) {
        // writeIndex: points to the position where we should write next character
        // index: points to the current character being processed
        int writeIndex = 0;
        int index = 0;

        // Process each character in the array
        while (index < chars.length) {
            // Store the current character for consecutive count
            char currentChar = chars[index];
            int count = 0;

            // Count consecutive occurrences of the current character
            // Key Logic: Keep moving index forward as long as we see the same character
            while (index < chars.length && chars[index] == currentChar) {
                index++;
                count++;
            }

            // Write the character at writeIndex position and increment writeIndex
            chars[writeIndex++] = currentChar;

            // If count > 1, we need to write the count after the character
            // Important: Handle multi-digit numbers correctly
            if (count > 1) {
                // Convert count to string to handle multi-digit numbers
                String countStr = String.valueOf(count);
                // Write each digit of the count individually
                for (char c : countStr.toCharArray()) {
                    chars[writeIndex++] = c;
                }
            }
        }

        // Return the length of the compressed array
        return writeIndex;
    }

    public static void main(String[] args) {
        // Test Case 1: Multiple characters with different frequencies
        // Expected output: "a2b2c3"
        System.out.println("Test Case 1:");
        char[] test1 = new char[]{ 'a','a','b','b','c','c','c' };
        System.out.println("Before compression: " + new String(test1));
        int result1 = compress(test1);
        System.out.println("Compressed length: " + result1);
        System.out.println("After compression: " + new String(test1, 0, result1));
        System.out.println();

        // Test Case 2: Single character
        // Expected output: "a"
        System.out.println("Test Case 2:");
        char[] test2 = new char[]{ 'a' };
        System.out.println("Before compression: " + new String(test2));
        int result2 = compress(test2);
        System.out.println("Compressed length: " + result2);
        System.out.println("After compression: " + new String(test2, 0, result2));
        System.out.println();

        // Test Case 3: Character with double-digit frequency
        // Expected output: "ab12"
        System.out.println("Test Case 3:");
        char[] test3 = new char[]{ 'a','b','b','b','b','b','b','b','b','b','b','b','b' };
        System.out.println("Before compression: " + new String(test3));
        int result3 = compress(test3);
        System.out.println("Compressed length: " + result3);
        System.out.println("After compression: " + new String(test3, 0, result3));
    }
}

```





