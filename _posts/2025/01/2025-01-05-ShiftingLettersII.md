---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2381. Shifting Letters II"
date: "2025-01-05"
tags: Medium
categories:
  - "LeetCode Array"
---


- You are given a string `s` of lowercase English letters and a 2D integer array `shifts` where `shifts[i] = [starti, endi, directioni]`. For every `i`, **shift** the characters in `s` from the index `starti` to the index `endi` (**inclusive**) forward if `directioni = 1`, or shift the characters backward if `directioni = 0`.
- Shifting a character **forward** means replacing it with the **next** letter in the alphabet (wrapping around so that `'z'` becomes `'a'`). Similarly, shifting a character **backward** means replacing it with the **previous** letter in the alphabet (wrapping around so that `'a'` becomes `'z'`).
- Return *the final string after all such shifts to* `s` *are applied*.

**Example 1**

```
Input: s = "abc", shifts = [[0,1,0],[1,2,1],[0,2,1]]
Output: "ace"
Explanation: Firstly, shift the characters from index 0 to index 1 backward. Now s = "zac".
Secondly, shift the characters from index 1 to index 2 forward. Now s = "zbd".
Finally, shift the characters from index 0 to index 2 forward. Now s = "ace".
```

**Example 2**

```
Input: s = "dztz", shifts = [[0,0,0],[1,1,1]]
Output: "catz"
Explanation: Firstly, shift the characters from index 0 to index 0 backward. Now s = "cztz".
Finally, shift the characters from index 1 to index 1 forward. Now s = "catz".
```

## Method 1

```tex
【O(n + m) time | O(n) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/01/05
 */
public class ShiftingLettersII {
    public static String shiftString(String s, int[][] shifts) {
        int n = s.length();
        // Create difference array with size n+1 to handle the boundary case
        // The extra position is used to mark the end of the last interval
        int[] diff = new int[n + 1];

        // First loop: Process all shift operations using difference array technique
        for (int[] shift : shifts) {
            int start = shift[0];    // Start index of current interval
            int end = shift[1];      // End index of current interval
            // Convert direction to actual shift value
            // If direction is 1 (forward), use 1; if direction is 0 (backward), use -1
            int direction = shift[2] == 1 ? 1 : -1;

            // Mark the start of interval with the shift value
            diff[start] += direction;
            // Mark the position after end of interval with opposite shift value
            // This ensures the shift effect stops after the interval
            diff[end + 1] -= direction;
        }

        // Create StringBuilder for efficient string construction
        StringBuilder result = new StringBuilder();
        // Track accumulated shift value for current position
        int currentShift = 0;

        // Second loop: Build the result string by applying accumulated shifts
        for (int i = 0; i < n; i++) {
            // Add current position's difference value to running sum
            // This gives us the total shift amount for current position
            currentShift += diff[i];

            // Get current character from input string
            char c = s.charAt(i);

            // Calculate new character position after shifting
            // 1. (c - 'a'): Convert character to 0-based position (a=0, b=1, ...)
            // 2. currentShift % 26: Ensure shift amount stays within alphabet range
            // 3. + 26: Handle negative shifts by adding full alphabet length
            // 4. % 26: Ensure final position stays within alphabet range (0-25)
            int shifted = ((c - 'a') + currentShift % 26 + 26) % 26;

            // Convert numerical position back to character and append to result
            result.append((char) (shifted + 'a'));
        }

        return result.toString();
    }

    public static void main(String[] args) {
        // Test Case 1: Multiple overlapping shifts
        String s1 = "abc";
        int[][] shifts1 = {
                {0, 1, 0},  // Shift index 0-1 backward
                {1, 2, 1},  // Shift index 1-2 forward
                {0, 2, 1}   // Shift index 0-2 forward
        };
        System.out.println("Test Case 1:");
        System.out.println("Input string: " + s1);
        System.out.println("Expected output: ace");
        System.out.println("Actual output: " + shiftString(s1, shifts1));

        // Test Case 2: Single character shifts
        String s2 = "dztz";
        int[][] shifts2 = {
                {0, 0, 0},  // Shift first character backward
                {1, 1, 1}   // Shift second character forward
        };
        System.out.println("\nTest Case 2:");
        System.out.println("Input string: " + s2);
        System.out.println("Expected output: catz");
        System.out.println("Actual output: " + shiftString(s2, shifts2));
    }
}

```





