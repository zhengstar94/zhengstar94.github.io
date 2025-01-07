---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1408. String Matching in an Array"
date: "2025-01-07"
tags: Easy
categories:
  - "LeetCode Array"
---


- Given an array of string `words`, return *all strings in* `words` *that is a **substring** of another word*. You can return the answer in **any order**.
- A **substring** is a contiguous sequence of characters within a string

**Example 1**

```
Input: words = ["mass","as","hero","superhero"]
Output: ["as","hero"]
Explanation: "as" is substring of "mass" and "hero" is substring of "superhero".
["hero","as"] is also a valid answer.
```

**Example 2**

```
Input: words = ["leetcode","et","code"]
Output: ["et","code"]
Explanation: "et", "code" are substring of "leetcode".
```

**Example 3**

```
Input: words = ["blue","green","bu"]
Output: []
Explanation: No string of words is substring of another string.
```

## Method 1

```tex
【O(n^2) time | O(k) space】
```

```java
package Leetcode.Array;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/01/07
 */
public class StringMatchingInAnArray {

  public static List<String> stringMatching(String[] words) {
    List<String> ans = new ArrayList<>();

    // Iterate through each word in the array
    for (int i = 0; i < words.length; i++) {
      // Compare current word with all other words
      for (int j = 0; j < words.length; j++) {
        // Skip if comparing word with itself
        if (i == j) {
          continue;
        }
        // Check if words[i] is a substring of words[j]
        if (words[j].contains(words[i])) {
          // Add words[i] to result and break inner loop since one match is sufficient
          ans.add(words[i]);
          break;
        }
      }
    }
    return ans;
  }

  public static void main(String[] args) {
    // Test case 1
    String[] test1 = {"mass", "as", "hero", "superhero"};
    System.out.println("Test case 1 result: " + stringMatching(test1));
    // Expected output: [as, hero]

    // Test case 2
    String[] test2 = {"leetcode", "et", "code"};
    System.out.println("Test case 2 result: " + stringMatching(test2));
    // Expected output: [et, code]

    // Test case 3
    String[] test3 = {"blue", "green", "bu"};
    System.out.println("Test case 3 result: " + stringMatching(test3));
    // Expected output: []
  }
}
```










