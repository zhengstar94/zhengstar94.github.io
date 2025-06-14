---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2566. Maximum Difference by Remapping a Digit"
date: "2025-06-14"
tags: Easy
categories:
  - "LeetCode Array"
---

- You are given an integer `num`. You know that Bob will sneakily **remap** one of the `10` possible digits (`0` to `9`) to another digit.
- Return *the difference between the maximum and minimum values Bob can make by remapping **exactly** **one** digit in* `num`.
- **Notes:**
  - When Bob remaps a digit d1 to another digit d2, Bob replaces all occurrences of `d1` in `num` with `d2`.
  - Bob can remap a digit to itself, in which case `num` does not change.
  - Bob can remap different digits for obtaining minimum and maximum values respectively.
  - The resulting number after remapping can contain leading zeroes.

**Example 1**

```
Input: num = 11891
Output: 99009
Explanation: 
To achieve the maximum value, Bob can remap the digit 1 to the digit 9 to yield 99899.
To achieve the minimum value, Bob can remap the digit 1 to the digit 0, yielding 890.
The difference between these two numbers is 99009.
```

**Example 2**

```
Input: num = 90
Output: 99
Explanation:
The maximum value that can be returned by the function is 99 (if 0 is replaced by 9) and the minimum value that can be returned by the function is 0 (if 9 is replaced by 0).
Thus, we return 99.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @Author zhengxingxing
 * @Date 2025/06/14
 */
public class MaximumDifferenceByRemappingADigit {
    
    public static int minMaxDifference(int num) {
        // Convert the input number to a string for easier manipulation.
        String s = Integer.toString(num);
        // Create a copy of the original string to manipulate for the minimum number.
        String t = s;

        // Initialize a position variable to find the first non-'9' digit.
        int pos = 0;
        // Loop through the string until we find a digit that is not '9'.
        while (pos < s.length() && s.charAt(pos) == '9') {
            pos++;
        }

        // If we found a non-'9' digit, replace it with '9' in the string 's'.
        if (pos < s.length()) {
            s = s.replace(s.charAt(pos), '9');
        }

        // Replace the first character of 't' with '0' to form the minimum number.
        t = t.replace(t.charAt(0), '0');

        // Parse the modified strings back to integers and return their difference.
        return Integer.parseInt(s) - Integer.parseInt(t);
    }

    public static void main(String[] args) {
        // Test cases to validate the functionality of the minMaxDifference method.
        System.out.println("Test Case 1:");
        System.out.println("Input: 11891");
        // Expected output: 99009 (by replacing '1' with '9' for max and '1' with '0' for min)
        System.out.println("Output: " + minMaxDifference(11891));

        System.out.println("\nTest Case 2:");
        System.out.println("Input: 90");
        // Expected output: 99 (by replacing '0' with '9' for max and '9' with '0' for min)
        System.out.println("Output: " + minMaxDifference(90));
    }
}

```





