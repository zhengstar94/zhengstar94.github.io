---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2900. Longest Unequal Adjacent Groups Subsequence I"
date: "2025-05-15"
tags: Easy
categories:
  - "LeetCode Array"
---


- You are given a string array `words` and a **binary** array `groups` both of length `n`, where `words[i]` is associated with `groups[i]`.
- Your task is to select the **longest alternating** subsequence from `words`. A subsequence of `words` is alternating if for any two consecutive strings in the sequence, their corresponding elements in the binary array `groups` differ. Essentially, you are to choose strings such that adjacent elements have non-matching corresponding bits in the `groups` array.
- Formally, you need to find the longest subsequence of an array of indices `[0, 1, ..., n - 1]` denoted as `[i0, i1, ..., ik-1]`, such that `groups[ij] != groups[ij+1]` for each `0 <= j < k - 1` and then find the words corresponding to these indices.
- Return *the selected subsequence. If there are multiple answers, return **any** of them.***
- **Note:** The elements in `words` are distinct.

**Example 1**

```
Input: words = ["e","a","b"], groups = [0,0,1]

Output: ["e","b"]

Explanation: A subsequence that can be selected is ["e","b"] because groups[0] != groups[2]. Another subsequence that can be selected is ["a","b"] because groups[1] != groups[2]. It can be demonstrated that the length of the longest subsequence of indices that satisfies the condition is 2.
```

**Example 2**

```
Input: words = ["a","b","c","d"], groups = [1,0,1,1]

Output: ["a","b","c"]

Explanation: A subsequence that can be selected is ["a","b","c"] because groups[0] != groups[1] and groups[1] != groups[2]. Another subsequence that can be selected is ["a","b","d"] because groups[0] != groups[1] and groups[1] != groups[3]. It can be shown that the length of the longest subsequence of indices that satisfies the condition is 3.
```

## Method 1

```tex
【O(n * log(max(nums) - min(nums))) time | O(1) space】
```

```java
package Leetcode.Array;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/05/15
 */
public class LongestUnequalAdjacentGroupsSubsequenceI {
    public static List<String> getLongestSubsequence(String[] words, int[] groups) {
        // Initialize the result list to store our subsequence
        List<String> result = new ArrayList<>();

        // Add the first element to the result
        result.add(words[0]);

        // Remember the group value of the last added element
        int lastGroup = groups[0];

        // Iterate through the remaining elements
        for (int i = 1; i < words.length; i++) {
            // If the current element's group is different from the last added element's group
            if (groups[i] != lastGroup) {
                // Add the current element to our result
                result.add(words[i]);

                // Update the last group value
                lastGroup = groups[i];
            }
        }

        // Return the longest subsequence
        return result;
    }

    public static void main(String[] args) {
        // Test Case 1
        String[] words1 = {"a", "b", "c", "d"};
        int[] groups1 = {1, 0, 1, 1};
        List<String> result1 = getLongestSubsequence(words1, groups1);
        System.out.println("Test Case 1:");
        System.out.println("Input: words = " + Arrays.toString(words1) + ", groups = " + Arrays.toString(groups1));
        System.out.println("Output: " + result1);
        System.out.println("Expected: [a, b, c] or [a, b, d]");
        System.out.println();

        // Test Case 2
        String[] words2 = {"e", "a", "b"};
        int[] groups2 = {0, 0, 1};
        List<String> result2 = getLongestSubsequence(words2, groups2);
        System.out.println("Test Case 2:");
        System.out.println("Input: words = " + Arrays.toString(words2) + ", groups = " + Arrays.toString(groups2));
        System.out.println("Output: " + result2);
        System.out.println("Expected: [e, b] or [a, b]");
        System.out.println();

        // Additional Test Case 3
        String[] words3 = {"red", "green", "blue", "yellow", "purple"};
        int[] groups3 = {0, 1, 0, 0, 1};
        List<String> result3 = getLongestSubsequence(words3, groups3);
        System.out.println("Test Case 3:");
        System.out.println("Input: words = " + Arrays.toString(words3) + ", groups = " + Arrays.toString(groups3));
        System.out.println("Output: " + result3);
        System.out.println("Expected: [red, green, blue, purple]");
    }
}

```





