---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2337. Move Pieces to Obtain a String"
date: "2025-03-19"
tags: Medium TwoPointers
categories:
  - "LeetCode DoubleSeqTwoPointers"
---


- You are given two strings `start` and `target`, both of length `n`. Each string consists **only** of the characters `'L'`, `'R'`, and `'_'` where:
  - The characters `'L'` and `'R'` represent pieces, where a piece `'L'` can move to the **left** only if there is a **blank** space directly to its left, and a piece `'R'` can move to the **right** only if there is a **blank** space directly to its right.
  - The character `'_'` represents a blank space that can be occupied by **any** of the `'L'` or `'R'` pieces.
- Return `true` *if it is possible to obtain the string* `target` *by moving the pieces of the string* `start` ***any** number of times*. Otherwise, return `false`.

**Example 1**

```
Input: start = "_L__R__R_", target = "L______RR"
Output: true
Explanation: We can obtain the string target from start by doing the following moves:
- Move the first piece one step to the left, start becomes equal to "L___R__R_".
- Move the last piece one step to the right, start becomes equal to "L___R___R".
- Move the second piece three steps to the right, start becomes equal to "L______RR".
Since it is possible to get the string target from start, we return true.
```

**Example 2**

```
Input: start = "R_L_", target = "__LR"
Output: false
Explanation: The 'R' piece in the string start can move one step to the right to obtain "_RL_".
After that, no pieces can move anymore, so it is impossible to obtain the string target from start.
```

**Example 3**

```
Input: start = "_R", target = "R_"
Output: false
Explanation: The piece in the string start can move only to the right, so it is impossible to obtain the string target from start.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqTwoPointers;

/**
 * @author zhengxingxing
 * @date 2025/03/19
 */
public class MovePiecesToObtainAString {

    public static boolean canChange(String start, String target) {
        // First check: Compare strings after removing all '_' characters
        // They must be identical to have any chance of transformation
        if (!start.replace("_", "").equals(target.replace("_", ""))) {
            return false;
        }

        // Use two pointers to compare non-'_' characters' relative positions
        for (int i = 0, j = 0; i < start.length(); i++) {
            // Skip '_' characters in start string
            if (start.charAt(i) == '_') {
                continue;
            }

            // Skip '_' characters in target string
            while (j < target.length() && target.charAt(j) == '_') {
                j++;
            }

            // Check if movement is valid:
            // 'L' cannot move right (i < j)
            // 'R' cannot move left (i > j)
            if ((start.charAt(i) == 'L' && i < j) ||
                    (start.charAt(i) == 'R' && i > j)) {
                return false;
            }
            j++;
        }
        return true;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Valid transformation
        String start1 = "_L__R__R_";
        String target1 = "L______RR";
        System.out.println("Test Case 1 Output: " + canChange(start1, target1)); // Expected: true

        // Test Case 2: Invalid transformation - crossing characters
        String start2 = "R_L_";
        String target2 = "__LR";
        System.out.println("Test Case 2 Output: " + canChange(start2, target2)); // Expected: false

        // Test Case 3: Invalid transformation - wrong movement
        String start3 = "_R";
        String target3 = "R_";
        System.out.println("Test Case 3 Output: " + canChange(start3, target3)); // Expected: false
    }
}

```





