---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3499. Maximize Active Section with Trade I"
date: "2025-04-22"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---


- You are given a binary string `s` of length `n`, where:
  - `'1'` represents an **active** section.
  - `'0'` represents an **inactive** section.
- You can perform **at most one trade** to maximize the number of active sections in `s`. In a trade, you:
  - Convert a contiguous block of `'1'`s that is surrounded by `'0'`s to all `'0'`s.
  - Afterward, convert a contiguous block of `'0'`s that is surrounded by `'1'`s to all `'1'`s.
- Return the **maximum** number of active sections in `s` after making the optimal trade.
- **Note:** Treat `s` as if it is **augmented** with a `'1'` at both ends, forming `t = '1' + s + '1'`. The augmented `'1'`s **do not** contribute to the final count.

**Example 1**

```
Input: s = "01"

Output: 1

Explanation:

Because there is no block of '1's surrounded by '0's, no valid trade is possible. The maximum number of active sections is 1.
```

**Example 2**

```
Input: s = "0100"

Output: 4

Explanation:

String "0100" → Augmented to "101001".
Choose "0100", convert "101001" → "100001" → "111111".
The final string without augmentation is "1111". The maximum number of active sections is 4.
```

**Example 3**

```
Input: s = "1000100"

Output: 7

Explanation:

String "1000100" → Augmented to "110001001".
Choose "000100", convert "110001001" → "110000001" → "111111111".
The final string without augmentation is "1111111". The maximum number of active sections is 7.
```

**Example 4**

```
Input: s = "01010"

Output: 4

Explanation:

String "01010" → Augmented to "1010101".
Choose "010", convert "1010101" → "1000101" → "1111101".
The final string without augmentation is "11110". The maximum number of active sections is 4.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/21
 */
public class MaximizeActiveSectionWithTradeI {
    public static int maxActiveSectionsAfterTrade(String s) {
        // Convert string to char array for easier manipulation
        char[] arr = s.toCharArray();
        int n = arr.length;
        int i = 0;

        // Variables to track:
        // total1: counts the total number of 1's in the original string
        // mx: stores the maximum convertible area (sum of two consecutive sequences of 0's)
        // pre0: stores the length of the previous sequence of 0's
        int total1 = 0;
        int mx = 0;
        // Initialize pre0 to MIN_VALUE to handle the first sequence of 0's
        int pre0 = Integer.MIN_VALUE;

        while (i < n) {
            // Mark the start of current consecutive sequence
            int start = i;

            // Find the end of current consecutive sequence of same characters
            // This is a grouped loop pattern to handle consecutive identical elements
            while (i < n && arr[i] == arr[start]) {
                i++;
            }

            // Calculate the length of current consecutive sequence
            int groupLength = i - start;

            // Process the current sequence based on its type (0's or 1's)
            if (arr[start] == '1') {
                // If current sequence is 1's, add its length to total count of 1's
                total1 += groupLength;
            } else {
                // If current sequence is 0's:
                // mx = Math.max(mx, pre0 + groupLength) finds the maximum convertible area
                // by comparing current maximum with sum of current and previous 0 sequences
                // This is crucial because we can potentially convert two adjacent 0 sequences into 1's
                mx = Math.max(mx, pre0 + groupLength);
                // Update pre0 for next iteration
                pre0 = groupLength;
            }
        }

        // Return the final result:
        // total1: original count of 1's
        // mx: maximum additional 1's we can get through one trade operation
        // The sum represents the maximum possible 1's after one trade
        return total1 + mx;
    }

    public static void main(String[] args) {
        // Test Case 1: Simple case with one 0 and one 1
        String s1 = "01";
        System.out.println("Test Case 1 Result: " + maxActiveSectionsAfterTrade(s1)); // Expected: 1

        // Test Case 2: Case where trade can convert multiple positions
        String s2 = "0100";
        System.out.println("Test Case 2 Result: " + maxActiveSectionsAfterTrade(s2)); // Expected: 4

        // Test Case 3: Case with longer consecutive 0's
        String s3 = "1000100";
        System.out.println("Test Case 3 Result: " + maxActiveSectionsAfterTrade(s3)); // Expected: 7

        // Test Case 4: Alternating 0's and 1's
        String s4 = "01010";
        System.out.println("Test Case 4 Result: " + maxActiveSectionsAfterTrade(s4)); // Expected: 4
    }
}

```





