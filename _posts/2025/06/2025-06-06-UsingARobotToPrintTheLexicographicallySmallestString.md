---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2434. Using a Robot to Print the Lexicographically Smallest String"
date: "2025-06-06"
tags: Medium
categories:
  - "LeetCode Greedy"
---

- You are given a string `s` and a robot that currently holds an empty string `t`. Apply one of the following operations until `s` and `t` **are both empty**:
  - Remove the **first** character of a string `s` and give it to the robot. The robot will append this character to the string `t`.
  - Remove the **last** character of a string `t` and give it to the robot. The robot will write this character on paper.

- Return *the lexicographically smallest string that can be written on the paper.*

**Example 1**

```
Input: s = "zza"
Output: "azz"
Explanation: Let p denote the written string.
Initially p="", s="zza", t="".
Perform first operation three times p="", s="", t="zza".
Perform second operation three times p="azz", s="", t="".
```

**Example 2**

```
Input: s = "bac"
Output: "abc"
Explanation: Let p denote the written string.
Perform first operation twice p="", s="c", t="ba". 
Perform second operation twice p="ab", s="c", t="". 
Perform first operation p="ab", s="", t="c". 
Perform second operation p="abc", s="", t="".
```

**Example 3**

```
Input: s = "bdda"
Output: "addb"
Explanation: Let p denote the written string.
Initially p="", s="bdda", t="".
Perform first operation four times p="", s="", t="bdda".
Perform second operation four times p="addb", s="", t="".
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Greedy;

import java.util.ArrayDeque;
import java.util.Deque;

/**
 * @Author zhengxingxing
 * @Date 2025/06/06
 */
public class UsingARobotToPrintTheLexicographicallySmallestString {

    public static String robotWithString(String s) {
        int n = s.length();

        // Step 1: Preprocessing - Build suffix minimum array
        // sufMin[i] represents the minimum character from position i to the end of string
        // This helps us decide whether to wait for a smaller character or output current one
        char[] sufMin = new char[n + 1];

        // Set sentinel value to avoid boundary checking
        // When we reach the end, any character is <= MAX_VALUE, so we can output everything
        sufMin[n] = Character.MAX_VALUE;

        // Build sufMin array from right to left
        // For each position i, sufMin[i] = min(current_char, min_of_remaining_suffix)
        for (int i = n - 1; i >= 0; i--) {
            sufMin[i] = (char) Math.min(sufMin[i + 1], s.charAt(i));
        }

        // Step 2: Main algorithm execution
        // Initialize result builder with initial capacity for better performance
        StringBuilder ans = new StringBuilder(n);

        // Use ArrayDeque instead of Stack for better performance
        // This stack simulates the temporary string 't' in the problem
        Deque<Character> st = new ArrayDeque<>();

        // Process each character in the input string
        for (int i = 0; i < n; i++) {
            // Operation 1: Take character from string s and put it into stack t
            // Always push the current character first
            st.push(s.charAt(i));

            // Operation 2: Decide when to pop characters from stack and write to paper
            // Key insight: Pop character if it's <= minimum character in remaining suffix
            // This ensures we output smaller characters as early as possible

            // sufMin[i + 1] represents minimum character from next position to end
            // If current stack top <= sufMin[i + 1], it means:
            // 1. Either no more characters left (sufMin[i+1] = MAX_VALUE), so output everything
            // 2. Or current character is not larger than any future character, safe to output
            while (!st.isEmpty() && st.peek() <= sufMin[i + 1]) {
                // Pop from stack and append to result (write to paper)
                ans.append(st.pop());
            }
        }

        // Convert StringBuilder to String and return
        // At this point, both s and stack t should be empty
        return ans.toString();
    }
    
    public static void main(String[] args) {

        // Test case 1: Example from problem statement
        String s1 = "zza";
        String result1 = robotWithString(s1);
        System.out.println("Output: " + result1);
        System.out.println("Expected: azz");
        System.out.println();

        // Test case 2: Another example from problem statement
        String s2 = "bac";
        String result2 = robotWithString(s2);
        System.out.println("Output: " + result2);
        System.out.println("Expected: abc");
        System.out.println();

        // Test case 3: Third example from problem statement
        String s3 = "bdda";
        String result3 = robotWithString(s3);
        System.out.println("Output: " + result3);
        System.out.println("Expected: addb");
        System.out.println();

        // Test case 4: Edge case - single character
        String s4 = "a";
        String result4 = robotWithString(s4);
        System.out.println("Output: " + result4);
        System.out.println("Expected: a");
        System.out.println();

        // Test case 5: Already sorted string
        String s5 = "abc";
        String result5 = robotWithString(s5);
        System.out.println("Output: " + result5);
        System.out.println("Expected: abc");
        System.out.println();

        // Test case 6: Reverse sorted string
        String s6 = "cba";
        String result6 = robotWithString(s6);
        System.out.println("Output: " + result6);
        System.out.println("Expected: abc");
    }
}
```





