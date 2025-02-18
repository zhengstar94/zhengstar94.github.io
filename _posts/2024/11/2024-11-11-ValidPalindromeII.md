---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "680.Valid Palindrome II"
date: "2024-11-11"
categories:
  - "LeetCode TwoPointers"
---

- Given a string `s`, return `true` *if the* `s` *can be palindrome after deleting **at most one** character from it*.

**Example 1**

```
Input: s = "aba"
Output: true
```

**Example 2**

```
Input: s = "abca"
Output: true
Explanation: You could delete the character 'c'.
```

**Example 3**

```
Input: s = "abc"
Output: false
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2024/11/11
 */
public class ValidPalindromeII {
    public static boolean validPalindrome(String s) {
        // Initialize two pointers, left starts from beginning, right starts from end
        int left = 0;
        int right = s.length() - 1;

        // Keep checking characters from both ends moving towards center
        while (left < right){
            // If characters at left and right pointers don't match
            if(s.charAt(left) != s.charAt(right)){
                // Try two possibilities:
                // 1. Remove character at left pointer (check substring from left+1 to right)
                // 2. Remove character at right pointer (check substring from left to right-1)
                return isPalindrome(s, left + 1, right) || isPalindrome(s, left, right - 1);
            }
            // Move pointers towards center if characters match
            left++;
            right--;
        }
        // If we get here, string is already a palindrome
        return true;
    }

    private static boolean isPalindrome(String s, int left, int right) {
        // Compare characters from both ends moving towards center
        while (left < right){
            if(s.charAt(left) != s.charAt(right)){
                return false;
            }
            // Move pointers towards center if characters match
            left++;
            right--;
        }
        // If we get here, substring is a palindrome
        return true;
    }

    public static void main(String[] args) {
        // Test case 1: Already a palindrome
        System.out.println(validPalindrome("aba"));  // Should print: true

        // Test case 2: Can become palindrome by removing one character
        System.out.println(validPalindrome("abca")); // Should print: true

        // Test case 3: Cannot become palindrome by removing one character
        System.out.println(validPalindrome("abd"));  // Should print: false
    }
}

```

