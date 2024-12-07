---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "58. Length of Last Word"
date: "2024-12-07"
tags: Easy
categories:
  - "LeetCode String"
---


- Given a string `s` consisting of words and spaces, return *the length of the **last** word in the string.*
- A **word** is a maximal substring consisting of non-space characters only.

**Example 1**

```
Input: s = "Hello World"
Output: 5
Explanation: The last word is "World" with length 5.
```

**Example 2**

```
Input: s = "   fly me   to   the moon  "
Output: 4
Explanation: The last word is "moon" with length 4.
```

**Example 2**

```
Input: s = "luffy is still joyboy"
Output: 6
Explanation: The last word is "joyboy" with length 6.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2024/12/07
 */
public class LengthOfLastWord {
    public static int lengthOfLastWord(String s) {
        int length = 0;

        for (int i = s.length() - 1; i >= 0; i--) {
            if(s.charAt(i) != ' '){
                length++;
            }else if(length > 0){
                break;
            }
        }
        return length;
    }

    public static void main(String[] args) {
        // Test case 1: Normal string with single space
        String test1 = "Hello World";
        System.out.println("Test 1: \"" + test1 + "\"");
        System.out.println("Length of last word: " + lengthOfLastWord(test1));

        // Test case 2: String with trailing spaces
        String test2 = "   fly me   to   the moon  ";
        System.out.println("\nTest 2: \"" + test2 + "\"");
        System.out.println("Length of last word: " + lengthOfLastWord(test2));

        // Test case 3: Single word
        String test3 = "luffy";
        System.out.println("\nTest 3: \"" + test3 + "\"");
        System.out.println("Length of last word: " + lengthOfLastWord(test3));

        // Test case 4: Empty string
        String test4 = "";
        System.out.println("\nTest 4: \"" + test4 + "\"");
        System.out.println("Length of last word: " + lengthOfLastWord(test4));
    }
}

```





