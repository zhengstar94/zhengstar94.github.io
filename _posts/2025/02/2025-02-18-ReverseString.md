---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "344. Reverse String"
date: "2025-02-18"
tags: Easy
categories:
  - "LeetCode TwoPointers"
---

- Write a function that reverses a string. The input string is given as an array of characters `s`.
- You must do this by modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) with `O(1)` extra memory.

**Example 1**

```
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

**Example 2**

```
Input: s = ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2025/02/18
 */
public class ReverseString {
    public static void reverseString(char[] s) {
        int n = s.length;
        int left = 0;
        int right = n - 1;

        // Swap characters from both ends until pointers meet in middle <sup data-citation="3" className="inline select-none [&>a]:rounded-2xl [&>a]:border [&>a]:px-1.5 [&>a]:py-0.5 [&>a]:transition-colors shadow [&>a]:bg-ds-bg-subtle [&>a]:text-xs [&>svg]:w-4 [&>svg]:h-4 relative -top-[2px] citation-shimmer"><a href="https://codegym.cc/groups/posts/1015-different-ways-to-reverse-a-string-in-java">3</a></sup>
        while(left < right) {
            // Swap characters at left and right pointers
            char tmp = s[left];
            s[left] = s[right];
            s[right] = tmp;

            left++;
            right--;
        }
    }

    public static void main(String[] args) {
        // Test case 1
        char[] str1 = "hello".toCharArray();
        System.out.print("Original string: ");
        System.out.println(str1);
        reverseString(str1);
        System.out.print("After reverse: ");
        System.out.println(str1);

        // Test case 2
        char[] str2 = "umbrella".toCharArray();
        System.out.print("Original string: ");
        System.out.println(str2);
        reverseString(str2);
        System.out.print("After reverse: ");
        System.out.println(str2);
    }
}

```





