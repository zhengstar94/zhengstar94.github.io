---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "670. Maximum Swap"
date: "2024-12-13"
tags: Medium
categories:
  - "LeetCode Array"
---

- You are given an integer `num`. You can swap two digits at most once to get the maximum valued number.
- Return *the maximum valued number you can get*.

**Example 1**

```
Input: num = 2736
Output: 7236
Explanation: Swap the number 2 and the number 7.
```

**Example 2**

```
Input: num = 9973
Output: 9973
Explanation: No swap.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2024/12/13
 */
public class MaximumSwap {
    public static int maximumSwap(int num) {
        // Convert the input number to a character array to easily manipulate individual digits
        char[] digits = Integer.toString(num).toCharArray();
        int n = digits.length; // Store the length of the character array

        // Array to store the last index of each digit (0-9) in the number
        int[] lastIndex = new int[10];

        // Iterate over the digits of the number and update the lastIndex array
        // lastIndex[digit] will store the rightmost index of that digit
        for (int i = 0; i < n; i++) {
            lastIndex[digits[i] - '0'] = i;  // The digit is converted from a character ('0' to '9') to an integer (0 to 9)
        }

        // Loop through each digit of the number to find a potential swap
        for (int i = 0; i < n; i++) {
            // Check if there is any larger digit to the right of the current digit
            // Start checking from digit 9 and go downwards (this ensures we check the largest possible digits first)
            for (int d = 9; d > digits[i] - '0'; d--) {
                // If a larger digit is found to the right of the current digit (i.e., lastIndex[d] > i)
                // Swap the digits to form the largest possible number
                if(lastIndex[d] > i){
                    // Swap the digits at the current position (i) and the last occurrence of the larger digit
                    char temp = digits[i];
                    digits[i] = digits[lastIndex[d]];
                    digits[lastIndex[d]] = temp;

                    // Convert the modified character array back to an integer and return the result
                    return Integer.parseInt(new String(digits));
                }
            }
        }

        // If no swap was needed, return the original number
        return num;
    }

    public static void main(String[] args) {
        // Test cases
        System.out.println(maximumSwap(2736)); // Output: 7236
        System.out.println(maximumSwap(9973)); // Output: 9973
        System.out.println(maximumSwap(1234)); // Output: 4231
        System.out.println(maximumSwap(98368)); // Output: 98863
    }
}

```





