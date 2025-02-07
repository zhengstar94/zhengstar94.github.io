---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1456. Maximum Number of Vowels in a Substring of Given Length"
date: "2024-12-19"
tags: Medium SlideWindow
categories:
  - "LeetCode SlideWindow"
---

- Given a string `s` and an integer `k`, return *the maximum number of vowel letters in any substring of* `s` *with length* `k`.
- **Vowel letters** in English are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.


**Example 1**

```
Input: s = "abciiidef", k = 3
Output: 3
Explanation: The substring "iii" contains 3 vowel letters.
```

**Example 2**

```
Input: s = "aeiou", k = 2
Output: 2
Explanation: Any substring of length 2 contains 2 vowels.
```

**Example 3**

```
Input: s = "leetcode", k = 3
Output: 2
Explanation: "lee", "eet" and "ode" contain 2 vowels.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2024/12/19
 */
public class MaximumNumberOfVowelsInASubstringOfGivenLength {
    public static int maxVowels(String S, int k) {
        char[] s = S.toCharArray();
        int ans = 0;
        int vowel = 0;

        for (int i = 0; i < s.length; i++) {
            // 1. Enter the window - check if current character is a vowel
            if (s[i] == 'a' || s[i] == 'e' || s[i] == 'i' || s[i] == 'o' || s[i] == 'u') {
                vowel++;
            }

            if (i < k - 1) { // Window size less than k, continue building the window
                continue;
            }

            ans = Math.max(ans, vowel);

            // 3. Exit the window - remove the leftmost character from count if it's a vowel
            char out = s[i - k + 1];
            if (out == 'a' || out == 'e' || out == 'i' || out == 'o' || out == 'u') {
                vowel--;
            }
        }
        return ans;
    }

    public static void main(String[] args) {

        // Test case 1: Normal case with mixed vowels and consonants
        String s1 = "abciiidef";
        int k1 = 3;
        System.out.println("Test case 1:");
        System.out.println("Input: s = " + s1 + ", k = " + k1);
        System.out.println("Output: " + maxVowels(s1, k1));  // Expected output: 3

        // Test case 2: String containing only vowels
        String s2 = "aeiou";
        int k2 = 2;
        System.out.println("\nTest case 2:");
        System.out.println("Input: s = " + s2 + ", k = " + k2);
        System.out.println("Output: " + maxVowels(s2, k2));  // Expected output: 2

        // Test case 3: String containing no vowels
        String s3 = "bcd";
        int k3 = 2;
        System.out.println("\nTest case 3:");
        System.out.println("Input: s = " + s3 + ", k = " + k3);
        System.out.println("Output: " + maxVowels(s3, k3));  // Expected output: 0

        // Test case 4: k equals string length
        String s4 = "leetcode";
        int k4 = 8;
        System.out.println("\nTest case 4:");
        System.out.println("Input: s = " + s4 + ", k = " + k4);
        System.out.println("Output: " + maxVowels(s4, k4));  // Expected output: 4
    }
}
```





