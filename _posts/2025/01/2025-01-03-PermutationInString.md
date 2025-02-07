---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "567.Permutation in String"
date: "2025-01-03"
tags: Medium SlideWindow
categories:
  - "LeetCode SlideWindow"
---

- Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.
- In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.


**Example 1**

```
Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").
```

**Example 2**

```
Input: s1 = "ab", s2 = "eidboaoo"
Output: false
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Stack;


/**
 * @author zhengstars
 * @date 2025/01/03
 */
public class PermutationInString {
  public static boolean checkInclusion(String s1, String s2) {
    // Get the lengths of the input strings s1 and s2
    int len1 = s1.length();
    int len2 = s2.length();

    // Arrays to store the frequency of characters in s1 and the current window of s2
    int[] count1 = new int[26]; // Frequency array for s1
    int[] count2 = new int[26]; // Frequency array for the sliding window in s2

    // Populate the frequency array for s1
    // Each character's frequency is incremented based on its position relative to 'a'
    for (int i = 0; i < len1; i++) {
      count1[s1.charAt(i) - 'a']++;
    }

    // Iterate through the characters of s2
    for (int i = 0; i < len2; i++) {
      // Add the current character in s2 to the frequency array for the sliding window
      count2[s2.charAt(i) - 'a']++;

      // When the sliding window reaches the size of s1 (len1), start processing
      if (i >= len1 - 1) {
        // Check if the character frequencies of the current window match those of s1
        if (matches(count1, count2)) {
          return true; // A valid permutation of s1 is found in s2
        }

        // Remove the leftmost character of the sliding window
        // This ensures the window size remains constant as we slide to the next position
        count2[s2.charAt(i - len1 + 1) - 'a']--;
      }
    }

    // If no matching permutation was found in s2, return false
    return false;
  }

  private static boolean matches(int[] count1, int[] count2) {
    // Compare the character frequency arrays for s1 and the current sliding window in s2
    for (int i = 0; i < 26; i++) {
      if (count1[i] != count2[i]) {
        return false; // Mismatch in character frequencies
      }
    }
    // All character frequencies match, indicating a valid permutation
    return true;
  }

  // Test cases
  public static void main(String[] args) {

    // Test case 1
    String s1 = "ab";
    String s2 = "eidbaooo";
    System.out.println("Test case 1: " + checkInclusion(s1, s2)); // Expected: true

    // Test case 2
    s1 = "ab";
    s2 = "eidboaoo";
    System.out.println("Test case 2: " + checkInclusion(s1, s2)); // Expected: false

    // Test case 3
    s1 = "adc";
    s2 = "dcda";
    System.out.println("Test case 3: " + checkInclusion(s1, s2)); // Expected: true

    // Test case 4
    s1 = "hello";
    s2 = "ooolleoooleh";
    System.out.println("Test case 4: " + checkInclusion(s1, s2)); // Expected: false

    // Test case 5
    s1 = "abcd";
    s2 = "abcd";
    System.out.println("Test case 5: " + checkInclusion(s1, s2)); // Expected: true
  }
}


```





