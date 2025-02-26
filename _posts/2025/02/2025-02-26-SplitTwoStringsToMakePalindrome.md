---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1616. Split Two Strings to Make Palindrome"
date: "2025-02-26"
tags: Medium
categories:
  - "LeetCode TwoPointers" 
---


- You are given two strings `a` and `b` of the same length. Choose an index and split both strings **at the same index**, splitting `a` into two strings: `aprefix` and `asuffix` where `a = aprefix + asuffix`, and splitting `b` into two strings: `bprefix` and `bsuffix` where `b = bprefix + bsuffix`. Check if `aprefix + bsuffix` or `bprefix + asuffix` forms a palindrome.
- When you split a string `s` into `sprefix` and `ssuffix`, either `ssuffix` or `sprefix` is allowed to be empty. For example, if `s = "abc"`, then `"" + "abc"`, `"a" + "bc"`, `"ab" + "c"` , and `"abc" + ""` are valid splits.
- Return `true` *if it is possible to form* *a palindrome string, otherwise return* `false`.
- **Notice** that `x + y` denotes the concatenation of strings `x` and `y`.

**Example 1**

```
Input: a = "x", b = "y"
Output: true
Explaination: If either a or b are palindromes the answer is true since you can split in the following way:
aprefix = "", asuffix = "x"
bprefix = "", bsuffix = "y"
Then, aprefix + bsuffix = "" + "y" = "y", which is a palindrome.
```

**Example 2**

```
Input: a = "xbdef", b = "xecab"
Output: false
```

**Example 3**

```
Input: a = "ulacfd", b = "jizalu"
Output: true
Explaination: Split them at index 3:
aprefix = "ula", asuffix = "cfd"
bprefix = "jiz", bsuffix = "alu"
Then, aprefix + bsuffix = "ula" + "alu" = "ulaalu", which is a palindrome.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2025/02/26
 */
public class SplitTwoStringsToMakePalindrome {

    /**
     * Main method to check if it's possible to form a palindrome by splitting and combining strings
     * This method tries both combinations:
     * 1. prefix of a + suffix of b
     * 2. prefix of b + suffix of a
     *
     * @param a First input string
     * @param b Second input string
     * @return true if a palindrome can be formed, false otherwise
     */
    public static boolean checkPalindromeFormation(String a, String b) {
        // Try both combinations and return true if either works
        return checkPalindrome(a, b) || checkPalindrome(b, a);
    }

    /**
     * Helper method to check if one combination can form a palindrome
     * Uses two-pointer technique to check characters from both ends
     *
     * Key algorithm steps:
     * 1. Match characters from both ends until mismatch is found
     * 2. If pointers cross or meet, we've found a palindrome
     * 3. Otherwise, check if remaining substring in either string is palindrome
     *
     * @param a String to use for prefix
     * @param b String to use for suffix
     * @return true if a palindrome can be formed
     */
    private static boolean checkPalindrome(String a, String b) {
        // Initialize two pointers: left starts from beginning, right from end
        int left = 0;
        int right = b.length() - 1;

        // First phase: Match characters from both ends
        // Continue while pointers haven't met and characters match
        while (left < right && a.charAt(left) == b.charAt(right)) {
            left++;
            right--;
        }

        // If pointers have met or crossed, we've found a palindrome
        // This means all characters matched successfully
        if (left >= right) {
            return true;
        }

        // Second phase: Check if remaining substring in either string is palindrome
        // We only need one of them to be palindrome to succeed
        return isPalindrome(a, left, right) || isPalindrome(b, left, right);
    }

    /**
     * Helper method to check if a substring is palindrome
     * Uses two-pointer technique to compare characters from both ends
     *
     * @param s String to check
     * @param left Start index of substring
     * @param right End index of substring
     * @return true if the substring is palindrome
     */
    private static boolean isPalindrome(String s, int left, int right) {
        // Continue checking while pointers haven't met
        while (left < right) {
            // If characters don't match, it's not a palindrome
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        // If we get here, the substring is palindrome
        return true;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Single character strings
        String a1 = "x";
        String b1 = "y";
        System.out.println("Test Case 1: " + checkPalindromeFormation(a1, b1)); // Expected: true

        // Test Case 2: No palindrome possible
        String a2 = "xbdef";
        String b2 = "xecab";
        System.out.println("Test Case 2: " + checkPalindromeFormation(a2, b2)); // Expected: false

        // Test Case 3: Palindrome possible with prefix/suffix combination
        String a3 = "ulacfd";
        String b3 = "jizalu";
        System.out.println("Test Case 3: " + checkPalindromeFormation(a3, b3)); // Expected: true

        // Test Case 4: Palindrome possible with partial matching
        String a4 = "abdef";
        String b4 = "fecab";
        System.out.println("Test Case 4: " + checkPalindromeFormation(a4, b4)); // Expected: true

        // Test Case 5: Palindrome possible with longer strings
        String a5 = "abadef";
        String b5 = "fedcba";
        System.out.println("Test Case 5: " + checkPalindromeFormation(a5, b5)); // Expected: true
    }
}

```





