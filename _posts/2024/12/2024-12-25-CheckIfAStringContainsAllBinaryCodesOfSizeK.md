---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1461. Check If a String Contains All Binary Codes of Size K"
date: "2024-12-25"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---

- Given a binary string `s` and an integer `k`, return `true` *if every binary code of length* `k` *is a substring of* `s`. Otherwise, return `false`.

**Example 1**

```
Input: s = "00110110", k = 2
Output: true
Explanation: The binary codes of length 2 are "00", "01", "10" and "11". They can be all found as substrings at indices 0, 1, 3 and 2 respectively.
```

**Example 2**

```
Input: s = "0110", k = 1
Output: true
Explanation: The binary codes of length 1 are "0" and "1", it is clear that both exist as a substring. 
```

**Example 3**

```
Input: s = "0110", k = 2
Output: false
Explanation: The binary code "00" is of length 2 and does not exist in the array.
```

## Method 1

```tex
【O(n) time | O(2^k) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2024/12/25
 */
public class CheckIfAStringContainsAllBinaryCodesOfSizeK {

    public static boolean hasAllCodes(String s, int k) {
        int n = s.length();
        // Early return if string length is less than k
        if (n < k) {
            return false;
        }

        // Total number of possible binary codes of length k is 2^k
        // For example, if k=2, need=4 (00,01,10,11)
        int need = 1 << k;
        // Array to mark which codes we've seen
        boolean[] seen = new boolean[need];
        // Count of unique codes found so far
        int count = 0;
        // Current value in the sliding window
        int windowVal = 0;

        // Process each character in the string
        for (int i = 0; i < n; i++) {
            // Step 1: Add new bit to window
            // Left shift to make room for new bit
            windowVal = windowVal << 1;
            // Add new bit to rightmost position
            windowVal = windowVal | (s.charAt(i) - '0');

            // Skip until window is fully formed (need k characters)
            if (i < k - 1) {
                continue;
            }

            // Step 2: Clean up window
            // Use mask to keep only k bits
            // For example, if k=2, mask=11 (binary)
            windowVal = windowVal & ((1 << k) - 1);

            // Step 3: Process current window
            // If this is a new code we haven't seen before
            if (!seen[windowVal]) {
                seen[windowVal] = true;
                count++;

                // If we've found all possible codes, can return early
                if (count == need) {
                    return true;
                }
            }
        }

        // Return whether we found all possible codes
        return count == need;
    }

    public static void main(String[] args) {

        // Test Case 1: s = "00110110", k = 2
        // Should return true as it contains all binary codes of length 2
//        String s1 = "00110110";
//        int k1 = 2;
//        System.out.println("Test Case 1:");
//        System.out.println("Input: s = " + s1 + ", k = " + k1);
//        System.out.println("Expected: true");
//        System.out.println("Output: " + hasAllCodes(s1, k1));
//        System.out.println();

        // Test Case 2: s = "00110", k = 2
        // Should return true as it contains all binary codes of length 2
        String s2 = "00110";
        int k2 = 2;
        System.out.println("Test Case 2:");
        System.out.println("Input: s = " + s2 + ", k = " + k2);
        System.out.println("Expected: true");
        System.out.println("Output: " + hasAllCodes(s2, k2));
        System.out.println();

        // Test Case 3: s = "0110", k = 2
        // Should return false as it's missing "00"
        String s3 = "0110";
        int k3 = 2;
        System.out.println("Test Case 3:");
        System.out.println("Input: s = " + s3 + ", k = " + k3);
        System.out.println("Expected: false");
        System.out.println("Output: " + hasAllCodes(s3, k3));
        System.out.println();

        // Test Case 4: s = "0000000001", k = 1
        // Should return true as it contains both "0" and "1"
        String s4 = "0000000001";
        int k4 = 1;
        System.out.println("Test Case 4:");
        System.out.println("Input: s = " + s4 + ", k = " + k4);
        System.out.println("Expected: true");
        System.out.println("Output: " + hasAllCodes(s4, k4));
    }
}

```





