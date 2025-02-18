---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "9. Palindrome Number"
date: "2024-12-06"
tags: Easy
categories:
  - "LeetCode TwoPointers"
---


- Given an integer `x`, return `true` *if* `x` *is a* ***palindrome****, and* `false` *otherwise*.

**Example 1**

```
Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.
```

**Example 2**

```
Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

**Example 3**

```
Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

## Method 1

```tex
【O(log(n)) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2024/12/06
 */
public class PalindromeNumber {
    public static boolean isPalindrome(int x) {
        // Negative numbers are not palindromes (e.g., -121).
        // Numbers ending in 0 are not palindromes unless the number is 0 itself.
        if (x < 0 || (x != 0 && x % 10 == 0)) {
            return false;
        }

        int reversedHalf = 0; // Stores the reversed second half of the number.

        // Reverses digits until half of the number is processed.
        // The loop continues as long as the original number is greater than the reversed half.
        while (x > reversedHalf) {
            // Extract the last digit of x and add it to reversedHalf.
            reversedHalf = reversedHalf * 10 + x % 10;
            // Remove the last digit from x.
            x /= 10;
        }

        // Compare the two halves of the number:
        // - If the number of digits is even, both halves should be equal (x == reversedHalf).
        // - If the number of digits is odd, the middle digit can be ignored by dividing reversedHalf by 10.
        return x == reversedHalf || x == reversedHalf / 10;
    }

    public static void main(String[] args) {
        // Test cases to validate the solution
        int test1 = 121;        // Palindrome
        int test2 = -121;       // Negative number, not a palindrome
        int test3 = 10;         // Ends in 0, not a palindrome
        int test4 = 12321;      // Odd-length palindrome
        int test5 = 123321;     // Even-length palindrome
        int test6 = 12345;      // Not a palindrome
        int test7 = 0;          // Palindrome, single digit
        int test8 = 1;          // Palindrome, single digit

        // Print test results
        System.out.println(test1 + " -> " + isPalindrome(test1)); // true
        System.out.println(test2 + " -> " + isPalindrome(test2)); // false
        System.out.println(test3 + " -> " + isPalindrome(test3)); // false
        System.out.println(test4 + " -> " + isPalindrome(test4)); // true
        System.out.println(test5 + " -> " + isPalindrome(test5)); // true
        System.out.println(test6 + " -> " + isPalindrome(test6)); // false
        System.out.println(test7 + " -> " + isPalindrome(test7)); // true
        System.out.println(test8 + " -> " + isPalindrome(test8)); // true
    }
}

```





