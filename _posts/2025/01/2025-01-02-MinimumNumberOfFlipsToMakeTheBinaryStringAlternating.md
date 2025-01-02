---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1888. Minimum Number of Flips to Make the Binary String Alternating"
date: "2025-01-02"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---


- You are given a binary string `s`. You are allowed to perform two types of operations on the string in any sequence:
  - **Type-1: Remove** the character at the start of the string `s` and **append** it to the end of the string.
  - **Type-2: Pick** any character in `s` and **flip** its value, i.e., if its value is `'0'` it becomes `'1'` and vice-versa.
- Return *the **minimum** number of **type-2** operations you need to perform* *such that* `s` *becomes **alternating**.*
- The string is called **alternating** if no two adjacent characters are equal.
  - For example, the strings `"010"` and `"1010"` are alternating, while the string `"0100"` is not.

**Example 1**

```
Input: s = "111000"
Output: 2
Explanation: Use the first operation two times to make s = "100011".
Then, use the second operation on the third and sixth elements to make s = "101010".
```

**Example 2**

```
Input: s = "010"
Output: 0
Explanation: The string is already alternating.
```

**Example 3**

```
Input: s = "1110"
Output: 1
Explanation: Use the second operation on the second element to make s = "1010".
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/02
 */
public class MinimumNumberOfFlipsToMakeTheBinaryStringAlternating {
    
    public static int minFlips(String s) {
        int n = s.length();
        // Duplicate the string to handle circular cases
        String doubled = s + s;

        // Build two target alternating patterns
        StringBuilder target1 = new StringBuilder(); // Starts with '0'
        StringBuilder target2 = new StringBuilder(); // Starts with '1'

        // Generate the patterns
        for (int i = 0; i < doubled.length(); i++) {
            target1.append(i % 2 == 0 ? '0' : '1');
            target2.append(i % 2 == 0 ? '1' : '0');
        }

        int ans = Integer.MAX_VALUE; // To store the minimum number of flips
        int diff1 = 0; // Differences with target1
        int diff2 = 0; // Differences with target2

        // Sliding window to calculate flips for each substring of length n
        for (int i = 0; i < n * 2; i++) {
            // Add the new character to the window
            diff1 += doubled.charAt(i) != target1.charAt(i) ? 1 : 0;
            diff2 += doubled.charAt(i) != target2.charAt(i) ? 1 : 0;

            // When the window size reaches n, calculate the minimum flips
            if (i >= n - 1) {
                ans = Math.min(ans, Math.min(diff1, diff2));

                // Remove the leftmost character from the window
                int leftIndex = i - n + 1;
                diff1 -= doubled.charAt(leftIndex) != target1.charAt(leftIndex) ? 1 : 0;
                diff2 -= doubled.charAt(leftIndex) != target2.charAt(leftIndex) ? 1 : 0;
            }
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test case 1
        String s1 = "111000";
        System.out.println("Test case 1: " + s1);
        System.out.println("Expected output: 2");
        System.out.println("Actual output: " + minFlips(s1));

        // Test case 2
        String s2 = "010";
        System.out.println("\nTest case 2: " + s2);
        System.out.println("Expected output: 0");
        System.out.println("Actual output: " + minFlips(s2));

        // Test case 3
        String s3 = "1110";
        System.out.println("\nTest case 3: " + s3);
        System.out.println("Expected output: 1");
        System.out.println("Actual output: " + minFlips(s3));
    }
}

```





