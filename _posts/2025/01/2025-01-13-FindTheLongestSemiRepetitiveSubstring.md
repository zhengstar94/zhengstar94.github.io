---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2730. Find the Longest Semi-Repetitive Substring"
date: "2025-01-13"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindowMax"
---


- You are given a digit string `s` that consists of digits from 0 to 9.
- A string is called **semi-repetitive** if there is **at most** one adjacent pair of the same digit. For example, `"0010"`, `"002020"`, `"0123"`, `"2002"`, and `"54944"` are semi-repetitive while the following are not: `"00101022"` (adjacent same digit pairs are 00 and 22), and `"1101234883"` (adjacent same digit pairs are 11 and 88).
- Return the length of the **longest semi-repetitive substring** of `s`.

**Example 1**

```
Input: s = "52233"
Output: 4

Explanation:

The longest semi-repetitive substring is "5223". Picking the whole string "52233" has two adjacent same digit pairs 22 and 33, but at most one is allowed.
```

**Example 2**

```
Input: s = "5494"
Output: 4

Explanation:

s is a semi-repetitive string.
```

**Example 3**

```
Input: s = "1111111"
Output: 2

Explanation:

The longest semi-repetitive substring is "11". Picking the substring "111" has two adjacent same digit pairs, but at most one is allowed.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/13
 */
public class FindTheLongestSemiRepetitiveSubstring {

    /**
     * This function finds the length of the longest "semi-repetitive" substring.
     * A "semi-repetitive" substring allows at most one pair of consecutive, identical characters.
     *
     * @param s The input string consisting of alphanumeric characters.
     * @return The length of the longest semi-repetitive substring.
     */
    public static int longestSemiRepetitiveSubstring(String s) {
        // If the input string length is less than or equal to 2, the substring will always have the same length
        if (s.length() <= 2) {
            return s.length();
        }

        // Variable to store the maximum length of the semi-repetitive substring we have found so far
        int maxLength = 0;

        // `start` is the beginning index of the current substring that satisfies the condition
        int start = 0;

        // `lastPair` stores the index of the first element of the most recent pair of consecutive identical characters
        int lastPair = -1;  // Initially set to -1 because no pair has been found yet

        // Traverse the string from the second character to the last character
        for (int i = 1; i < s.length(); i++) {
            // Check if the current character is equal to the previous character
            if (s.charAt(i) == s.charAt(i - 1)) {

                /**
                 * If this is the second occurrence of a consecutive pair (i.e., we already encountered a pair before):
                 * - We adjust `start` to the second element of the first consecutive pair (i.e., `lastPair + 1`).
                 * - This ensures the new substring being processed doesn't include more than one pair.
                 */
                if (lastPair != -1) {
                    start = lastPair + 1;
                }

                // Update `lastPair` to the current location of the first element in the new pair
                lastPair = i - 1;
            }

            /**
             * Calculate the length of the current semi-repetitive substring:
             * - It's the distance from `start` to the current index `i` (inclusive).
             * Update `maxLength` if this substring is longer.
             */
            maxLength = Math.max(maxLength, i - start + 1);
        }

        // Return the length of the longest semi-repetitive substring found
        return maxLength;
    }

    public static void main(String[] args) {
        // Test case 1: Input string contains two pairs of consecutive identical characters
        String s1 = "5224336";
        System.out.println("Test Case 1: " + s1);
        System.out.println("Expected Output: 4");
        System.out.println("Actual Output: " + longestSemiRepetitiveSubstring(s1));

        // Uncomment other test cases to validate the implementation:
        // Test case 2
        // String s2 = "5494";
        // System.out.println("\nTest Case 2: " + s2);
        // System.out.println("Expected Output: 4");
        // System.out.println("Actual Output: " + longestSemiRepetitiveSubstring(s2));
        //
        // // Test case 3
        // String s3 = "1111111";
        // System.out.println("\nTest Case 3: " + s3);
        // System.out.println("Expected Output: 2");
        // System.out.println("Actual Output: " + longestSemiRepetitiveSubstring(s3));
    }
}

```





