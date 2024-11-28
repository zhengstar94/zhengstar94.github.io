---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1047. Remove All Adjacent Duplicates In String"
date: "2024-11-28"
categories:
  - "LeetCode Stack"
---


- You are given a string `s` consisting of lowercase English letters. A **duplicate removal** consists of choosing two **adjacent** and **equal** letters and removing them.
- We repeatedly make **duplicate removals** on `s` until we no longer can.
- Return *the final string after all such duplicate removals have been made*. It can be proven that the answer is **unique**.

**Example 1**

```
Input: s = "abbaca"
Output: "ca"
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
```

**Example 2**

```
Input: s = "azxxzy"
Output: "ay"
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

/**
 * @author zhengxingxing
 * @date 2024/11/28
 */
public class RemoveAllAdjacentDuplicatesInString {
    public static String removeDuplicates(String s) {
        // Use StringBuilder as a stack to track characters
        StringBuilder stack = new StringBuilder();

        // Iterate through each character in the string
        for (char c: s.toCharArray()){
            // If stack is not empty and current char matches the top of the stack
            if(stack.length() > 0 && stack.charAt(stack.length() - 1) == c){
                // Remove the top character (eliminate adjacent duplicates)
                stack.deleteCharAt(stack.length() - 1);
            }else {
                // Otherwise, add the current character to the stack
                stack.append(c);
            }
        }

        // Return the final processed string
        return stack.toString();
    }

    public static void main(String[] args) {
        // Test case 1: Original example
        String test1 = "abbaca";
        System.out.println("Result 1: " + removeDuplicates(test1));

        // Test case 2: Another original example
        String test2 = "azxxzy";
        System.out.println("Result 2: " + removeDuplicates(test2));

        // Test case 3: All same characters
        String test3 = "aaaaaa";
        System.out.println("Result 3: " + removeDuplicates(test3));

        // Test case 4: No duplicate characters
        String test4 = "abcde";
        System.out.println("Result 4: " + removeDuplicates(test4));

        // Test case 5: Complex repeated pattern
        String test5 = "aaabbccddee";
        System.out.println("Result 5: " + removeDuplicates(test5));
    }
}

```





