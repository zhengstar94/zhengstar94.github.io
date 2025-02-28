---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "125. Valid Palindrome"
date: "2024-02-09"
tags: Easy TwoPointers
categories:
  - "LeetCode SingleSeqTwoPointersInward"
---


- A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.
- Given a string `s`, return `true` *if it is a **palindrome**, or* `false` *otherwise*.

**Example 1**

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

**Example 2**

```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

**Example 3**

```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengstars
 * @date 2024/02/26
 */
public class ValidPalindrome {
    public static boolean isPalindrome(String s) {
        // Store the length of the string
        int n = s.length();

        // Initialize two pointers: left starts from the beginning of the string; right starts from the end of the string
        int left = 0, right = n - 1;

        // Continue the process until left and right pointers meet
        while (left < right) {
            // Ignore all the non-alphanumeric characters from the beginning of the string
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                ++left;
            }
            // Ignore all the non-alphanumeric characters from the end of the string
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                --right;
            }
            // Compare the left and right characters while ignoring the case
            if (left < right) {
                if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
                    return false;
                }
                // If they are same, then move to the next pair of characters
                ++left;
                --right;
            }
        }
        // If there is no unmatched pair, then the string is a palindrome
        return true;
    }

    public static void main(String[] args) {

        // Test case 1: a string with non-alphanumeric characters
        String s1 = "A man, a plan, a canal: Panama";
        System.out.println("Test case 1 result: " + isPalindrome(s1)); // Output: true

        // Test case 2: a string which is not a palindrome
        String s2 = "apple";
        System.out.println("Test case 2 result: " + isPalindrome(s2)); // Output: false

        // Test case 3: a string which is space
        String s3 = " ";
        System.out.println("Test case 3 result: " + isPalindrome(s3)); // Output: true
    }
}

```

