---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1869. Longer Contiguous Segments of Ones than Zeros"
date: "2025-04-09"
tags: Easy
categories:
  - "LeetCode GroupedLoop"
---

- Given a binary string `s`, return `true` *if the **longest** contiguous segment of* `1`'*s is **strictly longer** than the **longest** contiguous segment of* `0`'*s in* `s`, or return `false` *otherwise*.
  - For example, in `s = "110100010"` the longest continuous segment of `1`s has length `2`, and the longest continuous segment of `0`s has length `3`.
- Note that if there are no `0`'s, then the longest continuous segment of `0`'s is considered to have a length `0`. The same applies if there is no `1`'s.

**Example 1**

```
Input: s = "1101"
Output: true
Explanation:
The longest contiguous segment of 1s has length 2: "1101"
The longest contiguous segment of 0s has length 1: "1101"
The segment of 1s is longer, so return true.
```

**Example 2**

```
Input: s = "111000"
Output: false
Explanation:
The longest contiguous segment of 1s has length 3: "111000"
The longest contiguous segment of 0s has length 3: "111000"
The segment of 1s is not longer, so return false.
```

**Example 3**

```
Input: s = "110100010"
Output: false
Explanation:
The longest contiguous segment of 1s has length 2: "110100010"
The longest contiguous segment of 0s has length 3: "110100010"
The segment of 1s is not longer, so return false.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/09
 */
public class LongerContiguousSegmentsOfOnesThanZeros {

    public static boolean checkZeroOnes(String s) {
        // Get the length of input string
        int n = s.length();

        // Variables to track the maximum consecutive counts of 1's and 0's
        int maxOnes = 0;
        int maxZeros = 0;

        // Index for traversing the string
        int i = 0;

        // Process the string using grouped loop pattern
        while (i < n) {
            // Mark the start position of current group
            int start = i;

            // Get the character that defines current group (either '0' or '1')
            char currentChar = s.charAt(start);

            // Continue while we see the same character
            while (i < n && s.charAt(i) == currentChar) {
                i++;
            }

            // Calculate the length of current continuous group
            int groupLength = i - start;

            // Update the appropriate maximum count based on current character
            if (currentChar == '1') {
                maxOnes = Math.max(maxOnes, groupLength);
            } else {
                maxZeros = Math.max(maxZeros, groupLength);
            }
        }

        // Return true if longest segment of 1's is strictly longer than 0's
        return maxOnes > maxZeros;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Basic case with mixed 1's and 0's
        String test1 = "1101";
        System.out.println("Test 1: " + test1);
        System.out.println("Expected result: true");
        System.out.println("Actual result: " + checkZeroOnes(test1));
        System.out.println();

        // Test Case 2: Equal length segments
        String test2 = "11000";
        System.out.println("Test 2: " + test2);
        System.out.println("Expected result: false");
        System.out.println("Actual result: " + checkZeroOnes(test2));
        System.out.println();

        // Test Case 3: Single character
        String test3 = "1";
        System.out.println("Test 3: " + test3);
        System.out.println("Expected result: true");
        System.out.println("Actual result: " + checkZeroOnes(test3));
        System.out.println();

        // Test Case 4: Alternating pattern
        String test4 = "101010";
        System.out.println("Test 4: " + test4);
        System.out.println("Expected result: false");
        System.out.println("Actual result: " + checkZeroOnes(test4));
        System.out.println();

        // Test Case 5: Long continuous sequence
        String test5 = "111000111111";
        System.out.println("Test 5: " + test5);
        System.out.println("Expected result: true");
        System.out.println("Actual result: " + checkZeroOnes(test5));
        System.out.println();
    }
}

```





