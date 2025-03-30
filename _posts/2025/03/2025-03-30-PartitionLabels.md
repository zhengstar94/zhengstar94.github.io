---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "763. Partition Labels"
date: "2025-03-30"
tags: Medium
categories:
  - "LeetCode Greedy"
---

- You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part. For example, the string `"ababcc"` can be partitioned into `["abab", "cc"]`, but partitions such as `["aba", "bcc"]` or `["ab", "ab", "cc"]` are invalid.
- Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.
- Return *a list of integers representing the size of these parts*.

**Example 1**

```
Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.
```

**Example 2**

```
Input: s = "eccbbbbdec"
Output: [10]
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Greedy;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/03/30
 */
public class PartitionLabels {

    public static List<Integer> partitionLabels(String s) {
        // Array to store the last position of each character (a-z)
        int[] lastPos = new int[26];

        // First pass: Record the last occurrence of each character
        for (int i = 0; i < s.length(); i++) {
            lastPos[s.charAt(i) - 'a'] = i;
        }

        // List to store the sizes of partitions
        List<Integer> result = new ArrayList<>();

        // Variables to track partition boundaries
        int start = 0;  // Start index of current partition
        int end = 0;    // End index of current partition

        // Second pass: Determine partition boundaries
        for (int i = 0; i < s.length(); i++) {
            // Update the end position of current partition
            // by taking the maximum of current end and last occurrence of current character
            end = Math.max(end, lastPos[s.charAt(i) - 'a']);

            // If we've reached the end of current partition
            if (i == end) {
                // Calculate partition size and add to result
                result.add(end - start + 1);
                // Update start position for next partition
                start = i + 1;
            }
        }

        return result;
    }
    
    public static void main(String[] args) {
        // Test Case 1: String with multiple partitions
        String s1 = "ababcbacadefegdehijhklij";
        System.out.println("Test Case 1 Result: " + partitionLabels(s1)); // Expected output: [9,7,8]

        // Test Case 2: String with single partition
        String s2 = "eccbbbbdec";
        System.out.println("Test Case 2 Result: " + partitionLabels(s2)); // Expected output: [10]

        // Test Case 3: String where each character forms its own partition
        String s3 = "abc";
        System.out.println("Test Case 3 Result: " + partitionLabels(s3)); // Expected output: [1,1,1]
    }
}

```





