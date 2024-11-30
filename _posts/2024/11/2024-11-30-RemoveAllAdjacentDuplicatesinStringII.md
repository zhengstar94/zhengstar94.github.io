---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1209. Remove All Adjacent Duplicates in String II"
date: "2024-11-29"
categories:
  - "LeetCode Stack"
---

- You are given a string `s` and an integer `k`, a `k` **duplicate removal** consists of choosing `k` adjacent and equal letters from `s` and removing them, causing the left and the right side of the deleted substring to concatenate together.
- We repeatedly make `k` **duplicate removals** on `s` until we no longer can.
- Return *the final string after all such duplicate removals have been made*. It is guaranteed that the answer is **unique**.

**Example 1**

```
Input: s = "abcd", k = 2
Output: "abcd"
Explanation: There's nothing to delete.
```

**Example 2**

```
Input: s = "deeedbbcccbdaa", k = 3
Output: "aa"
Explanation: 
First delete "eee" and "ccc", get "ddbbbdaa"
Then delete "bbb", get "dddaa"
Finally delete "ddd", get "aa"
```

**Example 3**

```
Input: s = "pbbcggttciiippooaais", k = 2
Output: "ps"
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.Collections;
import java.util.Stack;

/**
 * @author zhengxingxing
 * @date 2024/11/30
 */
public class RemoveAllAdjacentDuplicatesinStringII {

    static class Pair<K, V> {
        private K key;
        private V value;

        public Pair(K key, V value) {
            this.key = key;
            this.value = value;
        }

        public K getKey() { return key; }
        public V getValue() { return value; }
    }


    public static String removeDuplicates(String s, int k) {
        // Initialize a stack to store characters and their frequencies
        Stack<Pair<Character, Integer>> stack = new Stack<>();

        // Iterate through each character in the input string
        for (char c: s.toCharArray()) {
            // Check if the stack is not empty and the current character matches the top of the stack
            if (!stack.isEmpty() && stack.peek().getKey() == c) {
                // Get the current frequency of the character
                int count = stack.peek().getValue();

                // If the frequency is k-1, remove the entire block of k characters
                if (count == k - 1) {
                    stack.pop();
                }
                // Otherwise, increment the frequency
                else {
                    stack.pop();
                    stack.push(new Pair<>(c, count + 1));
                }
            }
            // If it's a new character or different from the top of the stack
            else {
                // Push the character with initial frequency of 1
                stack.push(new Pair<>(c, 1));
            }
        }

        // Rebuild the final string from the remaining characters in the stack
        StringBuilder result = new StringBuilder();
        for (Pair<Character, Integer> pair : stack) {
            // Use Collections.nCopies to repeat the character based on its frequency
            result.append(String.join("", Collections.nCopies(pair.getValue(), String.valueOf(pair.getKey()))));
        }

        return result.toString();
    }

    public static void main(String[] args) {
        // Test Case 1: Original example with multiple removals
        String s1 = "deeedbbcccbdaa";
        int k1 = 3;
        System.out.println("Test Case 1:");
        System.out.println("Output: " + removeDuplicates(s1, k1));
        System.out.println();

        // Test Case 2: No characters to remove
        String s2 = "abcd";
        int k2 = 2;
        System.out.println("Test Case 2:");
        System.out.println("Output: " + removeDuplicates(s2, k2));
        System.out.println();

        // Test Case 3: Multiple removal scenarios
        String s3 = "pbbcggttciiippooaais";
        int k3 = 2;
        System.out.println("Test Case 3:");
        System.out.println("Output: " + removeDuplicates(s3, k3));
        System.out.println();
    }
}

```





