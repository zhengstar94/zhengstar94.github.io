---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1750. Minimum Length of String After Deleting Similar Ends"
date: "2025-02-18"
tags: Medium TwoPointers
categories:
  - "LeetCode SingleSeqTwoPointersInward"
---



- Given a string `s` consisting only of characters `'a'`, `'b'`, and `'c'`. You are asked to apply the following algorithm on the string any number of times:
  1. Pick a **non-empty** prefix from the string `s` where all the characters in the prefix are equal.
  2. Pick a **non-empty** suffix from the string `s` where all the characters in this suffix are equal.
  3. The prefix and the suffix should not intersect at any index.
  4. The characters from the prefix and suffix must be the same.
  5. Delete both the prefix and the suffix.
- Return *the **minimum length** of* `s` *after performing the above operation any number of times (possibly zero times)*.

**Example 1**

```
Input: s = "ca"
Output: 2
Explanation: You can't remove any characters, so the string stays as is.
```

**Example 2**

```
Input: s = "cabaabac"
Output: 0
Explanation: An optimal sequence of operations is:
- Take prefix = "c" and suffix = "c" and remove them, s = "abaaba".
- Take prefix = "a" and suffix = "a" and remove them, s = "baab".
- Take prefix = "b" and suffix = "b" and remove them, s = "aa".
- Take prefix = "a" and suffix = "a" and remove them, s = "".
```

**Example 3**

```
Input: s = "aabccabba"
Output: 3
Explanation: An optimal sequence of operations is:
- Take prefix = "aa" and suffix = "a" and remove them, s = "bccabb".
- Take prefix = "b" and suffix = "bb" and remove them, s = "cca".
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2025/02/18
 */
public class MinimumLengthOfStringAfterDeletingSimilarEnds {
    public static int minimumLength(String s) {
        // Initialize two pointers
        int left = 0;
        int right = s.length() - 1;

        // Continue while pointers haven't met and characters at both ends are same
        while (left < right && s.charAt(left) == s.charAt(right)) {
            char current = s.charAt(left);

            // Move left pointer while same character continues
            while (left <= right && s.charAt(left) == current) {
                left++;
            }

            // Move right pointer while same character continues
            while (left <= right && s.charAt(right) == current) {
                right--;
            }
        }

        // Return remaining length
        return right - left + 1;
    }

    public static void main(String[] args) {
        // Test case 1
        String s1 = "ca";
        System.out.println("Test case 1: " + s1);
        System.out.println("Result: " + minimumLength(s1));  // Expected output: 2

        // Test case 2
        String s2 = "cabaabac";
        System.out.println("\nTest case 2: " + s2);
        System.out.println("Result: " + minimumLength(s2));  // Expected output: 0

        // Test case 3
        String s3 = "aabccabba";
        System.out.println("\nTest case 3: " + s3);
        System.out.println("Result: " + minimumLength(s3));  // Expected output: 3

        // Additional test case
        String s4 = "bbbbbbbbbbbbbbbbbbb";
        System.out.println("\nTest case 4: " + s4);
        System.out.println("Result: " + minimumLength(s4));  // Expected output: 0
    }

}

```





