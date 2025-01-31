---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "541. Reverse String II"
date: "2025-01-31"
tags: Medium
categories:
  - "LeetCode Two Pointers"
---

- Given a string `s` and an integer `k`, reverse the first `k` characters for every `2k` characters counting from the start of the string.
- If there are fewer than `k` characters left, reverse all of them. If there are less than `2k` but greater than or equal to `k` characters, then reverse the first `k` characters and leave the other as original.


**Example 1**

```
Input: s = "abcdefg", k = 2
Output: "bacdfeg"
```

**Example 2**

```
Input: s = "abcd", k = 2
Output: "bacd"
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2025/01/31
 */
public class ReverseStringII {
    public static String reverseStr(String s, int k) {
        char[] arr = s.toCharArray();

        for (int i = 0; i < arr.length; i += 2 * k) {
            // Calculate the actual length that needs to be reversed each time
            int left = i;
            // Use Math.min to handle boundary cases when remaining chars less than k
            int right = Math.min(i + k - 1, arr.length - 1);

            // Two pointers approach to reverse characters
            while (left < right){
                char temp = arr[left];
                arr[left] = arr[right];
                arr[right] = temp;
                left++;
                right--;
            }
        }
        return new String(arr);
    }

    public static void main(String[] args) {
        // Test case 1: Regular case
        String s1 = "abcdefg";
        int k1 = 2;
        System.out.println("Test case 1 result: " + reverseStr(s1, k1)); // Expected output: "bacdfeg"

        // Test case 2: String length equals 2k
        String s2 = "abcd";
        int k2 = 2;
        System.out.println("Test case 2 result: " + reverseStr(s2, k2)); // Expected output: "bacd"

        // Test case 3: Edge case - single character
        String s3 = "a";
        int k3 = 2;
        System.out.println("Test case 3 result: " + reverseStr(s3, k3)); // Expected output: "a"
    }
}

```





