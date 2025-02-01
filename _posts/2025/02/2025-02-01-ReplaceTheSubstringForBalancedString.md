---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1234. Replace the Substring for Balanced String"
date: "2025-02-01"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindowMin"
---

- You are given a string s of length `n` containing only four kinds of characters: `'Q'`, `'W'`, `'E'`, and `'R'`.
- A string is said to be **balanced** if each of its characters appears `n / 4` times where `n` is the length of the string.
- Return *the minimum length of the substring that can be replaced with **any** other string of the same length to make* `s` ***balanced***. If s is already **balanced**, return `0`.

**Example 1**

```
Input: s = "QWER"
Output: 0
Explanation: s is already balanced.
```

**Example 2**

```
Input: s = "QQWE"
Output: 1
Explanation: We need to replace a 'Q' to 'R', so that "RQWE" (or "QRWE") is balanced.
```

**Example 3**

```
Input: s = "QQQW"
Output: 2
Explanation: We can replace the first "QQ" to "ER". 
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowMin;

/**
 * @author zhengxingxing
 * @date 2025/02/01
 */
public class ReplaceTheSubstringForBalancedString {

    public static int balancedString(String s) {
        // Array to store character frequency
        // Using ASCII array instead of HashMap for better performance
        int[] count = new int[128];

        // Count initial frequency of each character in the string
        for (char c : s.toCharArray()) {
            count[c]++;
        }

        // Calculate target frequency for each character
        // For a balanced string, each character should appear exactly n/4 times
        int n = s.length();
        int target = n / 4;

        // Check if string is already balanced
        // If each character appears exactly target times, return 0
        if (count['Q'] == target && count['W'] == target &&
                count['E'] == target && count['R'] == target) {
            return 0;
        }

        // Initialize minimum length as string length
        // This is the maximum possible length we might need to replace
        int minLen = n;

        // Initialize left pointer for sliding window
        int left = 0;

        // Sliding window implementation
        // Right pointer moves through the string
        for (int right = 0; right < n; right++) {
            // Decrease count of current character
            // This character is now inside our window
            count[s.charAt(right)]--;

            // Try to minimize window size while maintaining valid condition
            while (left <= right && // Ensure window is valid
                    count['Q'] <= target && // Check if Q outside window <= target
                    count['W'] <= target && // Check if W outside window <= target
                    count['E'] <= target && // Check if E outside window <= target
                    count['R'] <= target) { // Check if R outside window <= target

                // Update minimum length if current window is smaller
                // right - left + 1 gives current window size
                minLen = Math.min(minLen, right - left + 1);

                // Move left pointer by one position
                // Add the character at left pointer back to count
                // (it's now outside the window)
                count[s.charAt(left)]++;

                // Move left pointer to try to find smaller valid window
                left++;
            }
        }

        return minLen;
    }

    /**
     * Main method to test the solution with various test cases
     */
    public static void main(String[] args) {
        // Test Case 1: Already balanced string
        // Expected output: 0 (no replacement needed)
        System.out.println("Test case 1: " + balancedString("QWER"));

        // Test Case 2: Need to replace one character
        // String "QQWE" needs one character replacement to be balanced
        // Expected output: 1
        System.out.println("Test case 2: " + balancedString("QQWE"));

        // Test Case 3: Need to replace two characters
        // Expected output: 2 (replace "QQ" with "ER")
        System.out.println("Test case 3: " + balancedString("QQQW"));

        // Test Case 4: Longer string test
        // Expected output: 3
        System.out.println("Test case 4: " + balancedString("QQQWEERR"));

        // Test Case 5: All same characters
        // Expected output: 3 (need to replace three Q's)
        System.out.println("Test case 5: " + balancedString("QQQQ"));
    }
}

```





