---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1432. Max Difference You Can Get From Changing an Integer"
date: "2025-06-15"
tags: Medium
categories:
  - "LeetCode Greedy"
---


- You are given an integer `num`. You will apply the following steps to `num` **two** separate times:
  - Pick a digit `x (0 <= x <= 9)`.
  - Pick another digit `y (0 <= y <= 9)`. Note `y` can be equal to `x`.
  - Replace all the occurrences of `x` in the decimal representation of `num` by `y`.
- Let `a` and `b` be the two results from applying the operation to `num` *independently*.
- Return *the max difference* between `a` and `b`.
- Note that neither `a` nor `b` may have any leading zeros, and **must not** be 0.

**Example 1**

```
Input: num = 555
Output: 888
Explanation: The first time pick x = 5 and y = 9 and store the new integer in a.
The second time pick x = 5 and y = 1 and store the new integer in b.
We have now a = 999 and b = 111 and max difference = 888
```

**Example 2**

```
Input: num = 9
Output: 8
Explanation: The first time pick x = 9 and y = 9 and store the new integer in a.
The second time pick x = 9 and y = 1 and store the new integer in b.
We have now a = 9 and b = 1 and max difference = 8
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Greedy;

/**
 * @Author zhengxingxing
 * @Date 2025/06/15
 */
public class MaxDifferenceYouCanGetFromChangingAnInteger {

    public static int maxDiff(int num) {
        // Convert the integer to its string representation for easy digit manipulation.
        String s = String.valueOf(num);
        char[] cs = s.toCharArray();

        // Step 1: Find the maximum possible value (mx)
        // Start with the original number as the default maximum.
        int mx = num;
        // Iterate through each digit to find the first digit that is not '9'.
        // Replace all occurrences of this digit with '9' to maximize the number.
        for (char d : cs) {
            if (d != '9') {
                mx = replace(s, d, '9');
                break; // Only replace the first such digit found.
            }
        }

        // Step 2: Find the minimum possible value (mn)
        // Start with the original number as the default minimum.
        int mn = num;
        // If the first digit is not '1', replace all its occurrences with '1' to minimize the number
        // (while avoiding leading zeros).
        if (cs[0] != '1') {
            mn = replace(s, cs[0], '1');
        } else {
            // If the first digit is '1', look for the first digit (from the second position onward)
            // that is greater than '1' (i.e., not '0' or '1').
            // Replace all its occurrences with '0' to minimize the number (again, avoiding leading zeros).
            for (int i = 1; i < cs.length; i++) {
                if (cs[i] > '1') { // Not '0' and not '1'
                    mn = replace(s, cs[i], '0');
                    break; // Only replace the first such digit found.
                }
            }
        }

        // Step 3: Return the difference between the maximum and minimum values obtained.
        return mx - mn;
    }
    
    private static int replace(String s, char oldChar, char newChar) {
        // Replace all occurrences of oldChar with newChar.
        String t = s.replace(oldChar, newChar);
        // Convert the modified string back to an integer.
        return Integer.parseInt(t);
    }

    public static void main(String[] args) {
        // Test case 1: Input 555
        System.out.println("Test case 1: Input 555");
        System.out.println("Output: " + maxDiff(555));  // Expected output: 888

        // Test case 2: Input 9
        System.out.println("\nTest case 2: Input 9");
        System.out.println("Output: " + maxDiff(9));    // Expected output: 8

        // Test case 3: Input 123456
        System.out.println("\nTest case 3: Input 123456");
        System.out.println("Output: " + maxDiff(123456));  // Expected output: 820000

        // Test case 4: Input 10001
        System.out.println("\nTest case 4: Input 10001");
        System.out.println("Output: " + maxDiff(10001));   // Expected output: 80008
    }
}

```





