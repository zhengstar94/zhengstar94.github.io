---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2559. Count Vowel Strings in Ranges"
date: "2025-01-02"
tags: Medium
categories:
  - "LeetCode Math"
---


- You are given a **0-indexed** array of strings `words` and a 2D array of integers `queries`.
- Each query `queries[i] = [li, ri]` asks us to find the number of strings present in the range `li` to `ri` (both **inclusive**) of `words` that start and end with a vowel.
- Return *an array* `ans` *of size* `queries.length`*, where* `ans[i]` *is the answer to the* `i`th *query*.
- **Note** that the vowel letters are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

**Example 1**

```
Input: words = ["aba","bcb","ece","aa","e"], queries = [ [ 0,2],[1,4],[1,1 ] ]
Output: [2,3,0]
Explanation: The strings starting and ending with a vowel are "aba", "ece", "aa" and "e".
The answer to the query [0,2] is 2 (strings "aba" and "ece").
to query [1,4] is 3 (strings "ece", "aa", "e").
to query [1,1] is 0.
We return [2,3,0].
```

**Example 2**

```
Input: words = ["a","e","i"], queries = [ [ 0,2],[0,1],[2,2 ] ]
Output: [3,2,1]
Explanation: Every string satisfies the conditions, so we return [3,2,1].
```

## Method 1

```tex
【O(n + q) time | O(n) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/01/02
 */
public class CountVowelStringsInRanges {
    // Helper method to check if a character is a vowel
    private static boolean isVowel(char c) {
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
    }

    // Main method to process queries and return results
    public static int[] vowelStrings(String[] words, int[][] queries) {
        int n = words.length;
        // Create prefix sum array with size n+1
        // We use n+1 to handle the case when query starts from index 0
        int[] prefixSum = new int[n + 1];

        // Calculate prefix sum
        // For each position i, store the count of valid words from index 0 to i-1
        for (int i = 0; i < n; i++) {
            String word = words[i];
            // Check if current word starts and ends with vowels
            boolean isValid = !word.isEmpty() &&
                    isVowel(word.charAt(0)) &&
                    isVowel(word.charAt(word.length() - 1));
            // Add current word's value (1 if valid, 0 if not) to previous sum
            prefixSum[i + 1] = prefixSum[i] + (isValid ? 1 : 0);
        }

        // Process each query
        int[] result = new int[queries.length];
        for (int i = 0; i < queries.length; i++) {
            int left = queries[i][0];   // Query start index
            int right = queries[i][1];  // Query end index
            // Calculate number of valid words in range using prefix sum
            // right + 1 gets the sum up to right position
            // left gets the sum before the left position
            // Their difference gives the count in the range [left, right]
            result[i] = prefixSum[right + 1] - prefixSum[left];
        }

        return result;
    }

    public static void main(String[] args) {
        // Test case 1
        String[] words1 = {"aba", "bcb", "ece", "aa", "e"};
        int[][] queries1 = { {0,2}, {1,4}, {1,1} };
        int[] result1 = vowelStrings(words1, queries1);
        System.out.println("Test case 1 result: " + java.util.Arrays.toString(result1));

        // Test case 2
        String[] words2 = {"a", "e", "i"};
        int[][] queries2 = { {0,2}, {0,1}, {2,2} };
        int[] result2 = vowelStrings(words2, queries2);
        System.out.println("Test case 2 result: " + java.util.Arrays.toString(result2));
    }
}

```





