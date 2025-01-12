---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2116. Check if a Parentheses String Can Be Valid"
date: "2025-01-12"
tags: Medium
categories:
  - "LeetCode String"
---


- A parentheses string is a **non-empty** string consisting only of `'('` and `')'`. It is valid if **any** of the following conditions is **true**:
  - It is `()`.
  - It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid parentheses strings.
  - It can be written as `(A)`, where `A` is a valid parentheses string.
- You are given a parentheses string `s` and a string `locked`, both of length `n`. `locked` is a binary string consisting only of `'0'`s and `'1'`s. For **each** index `i` of `locked`,
  - If `locked[i]` is `'1'`, you **cannot** change `s[i]`.
  - But if `locked[i]` is `'0'`, you **can** change `s[i]` to either `'('` or `')'`.
- Return `true` *if you can make `s` a valid parentheses string*. Otherwise, return `false`.

**Example 1**

```
Input: s = "))()))", locked = "010100"
Output: true
Explanation: locked[1] == '1' and locked[3] == '1', so we cannot change s[1] or s[3].
We change s[0] and s[4] to '(' while leaving s[2] and s[5] unchanged to make s valid.
```

**Example 2**

```
Input: s = "()()", locked = "0000"
Output: true
Explanation: We do not need to make any changes because s is already valid.
```

**Example 3**

```
Input: s = ")", locked = "0"
Output: false
Explanation: locked permits  us to change s[0]. 
Changing s[0] to either '(' or ')' will not make s valid.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.String;

/**
 * @author zhengxingxing
 * @date 2025/01/12
 */
public class CheckIfAParenthesesStringCanBeValid {
    public static boolean canBeValid(String s, String locked) {
        // If the length is odd, it's impossible to form valid parentheses
        if (s.length() % 2 != 0) {
            return false;
        }

        // First pass: Left to right scan to check for excess right parentheses
        // balance: tracks the difference between left and right parentheses for locked positions
        // wild: counts the number of modifiable positions that can be used to fix imbalances
        int balance = 0, wild = 0;

        // Scan from left to right to ensure we never have too many right parentheses
        // This pass ensures that at each position, we can convert enough characters
        // to left parentheses to match any right parentheses we've seen
        for (int i = 0; i < s.length(); i++) {
            if (locked.charAt(i) == '0') {
                // Position is modifiable, increment wild counter
                wild++;
            } else if (s.charAt(i) == '(') {
                // Locked left parenthesis, increment balance
                balance++;
            } else {
                // Locked right parenthesis, decrement balance
                balance--;
            }

            // If balance + wild < 0, we have too many right parentheses
            // that cannot be fixed even using all modifiable positions
            if (balance + wild < 0) {
                return false;
            }
        }

        // Second pass: Right to left scan to check for excess left parentheses
        // Reset counters for the reverse scan
        balance = 0;
        wild = 0;

        // Scan from right to left to ensure we never have too many left parentheses
        // This pass ensures that at each position (working backwards),
        // we can convert enough characters to right parentheses to match any left
        // parentheses we've seen
        for (int i = s.length() - 1; i >= 0; i--) {
            if (locked.charAt(i) == '0') {
                // Position is modifiable, increment wild counter
                wild++;
            } else if (s.charAt(i) == ')') {
                // Locked right parenthesis, increment balance
                balance++;
            } else {
                // Locked left parenthesis, decrement balance
                balance--;
            }

            // If balance + wild < 0, we have too many left parentheses
            // that cannot be fixed even using all modifiable positions
            if (balance + wild < 0) {
                return false;
            }
        }

        // If both passes succeed, the string can be made valid
        return true;
    }

    public static void main(String[] args) {
        // Test Case 1: String can be made valid by modifying unlocked positions
        String s1 = "))()))";
        String locked1 = "010100";
        System.out.println("Test Case 1: " + canBeValid(s1, locked1));  // Expected output: true

        // Test Case 2: Already valid string with all positions modifiable
        String s2 = "()()";
        String locked2 = "0000";
        System.out.println("Test Case 2: " + canBeValid(s2, locked2));  // Expected output: true

        // Test Case 3: Single character cannot form valid parentheses
        String s3 = ")";
        String locked3 = "0";
        System.out.println("Test Case 3: " + canBeValid(s3, locked3));  // Expected output: false

        // Test Case 4: Additional test case with all positions modifiable
        String s4 = "())";
        String locked4 = "000";
        System.out.println("Test Case 4: " + canBeValid(s4, locked4));  // Expected output: true
    }
}

```





