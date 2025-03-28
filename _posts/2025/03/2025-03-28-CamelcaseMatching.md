---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1023. Camelcase Matching"
date: "2025-03-28"
tags: Medium TwoPointers
categories:
  - "LeetCode DoubleSeqSubsequencePointers"
---

- Given an array of strings `queries` and a string `pattern`, return a boolean array `answer` where `answer[i]` is `true` if `queries[i]` matches `pattern`, and `false` otherwise.
- A query word `queries[i]` matches `pattern` if you can insert lowercase English letters into the pattern so that it equals the query. You may insert a character at any position in pattern or you may choose not to insert any characters **at all**.

**Example 1**

```
Input: queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FB"
Output: [true,false,true,true,false]
Explanation: "FooBar" can be generated like this "F" + "oo" + "B" + "ar".
"FootBall" can be generated like this "F" + "oot" + "B" + "all".
"FrameBuffer" can be generated like this "F" + "rame" + "B" + "uffer".
```

**Example 2**

```
Input: queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FoBa"
Output: [true,false,true,false,false]
Explanation: "FooBar" can be generated like this "Fo" + "o" + "Ba" + "r".
"FootBall" can be generated like this "Fo" + "ot" + "Ba" + "ll".
```

**Example 3**

```
Input: queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FoBaT"
Output: [false,true,false,false,false]
Explanation: "FooBarTest" can be generated like this "Fo" + "o" + "Ba" + "r" + "T" + "est".
```

## Method 1

```tex
【O(n * m) time | O(n) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqSubsequencePointers;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/03/28
 */
public class CamelcaseMatching {

    public static List<Boolean> camelMatch(String[] queries, String pattern) {
        // Initialize result list to store matching results
        List<Boolean> result = new ArrayList<>();

        // Check each query string against the pattern
        for (String query: queries) {
            result.add(isMatch(query, pattern));
        }
        return result;
    }

    private static boolean isMatch(String query, String pattern) {
        // Initialize pattern pointer
        int j = 0;

        // Iterate through each character in query
        for (int i = 0; i < query.length(); i++) {
            // Case 1: Current characters match
            if (j < pattern.length() && query.charAt(i) == pattern.charAt(j)) {
                j++; // Move pattern pointer forward
            }
            // Case 2: Current query character is uppercase but doesn't match
            else if (Character.isUpperCase(query.charAt(i))) {
                return false; // Uppercase must match pattern
            }
            // Case 3: Current query character is lowercase and doesn't match
            // Simply continue to next character (implicit)
        }

        // Return true only if we've matched all characters in pattern
        return j == pattern.length();
    }

    public static void main(String[] args) {
        // Test case 1: Pattern "FB"
        String[] queries1 = {"FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"};
        String pattern1 = "FB";
        List<Boolean> result1 = camelMatch(queries1, pattern1);
        System.out.println("Test case 1 result: " + result1);

        // Test case 2: Pattern "FoBa"
        String[] queries2 = {"FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"};
        String pattern2 = "FoBa";
        List<Boolean> result2 = camelMatch(queries2, pattern2);
        System.out.println("Test case 2 result: " + result2);

        // Test case 3: Pattern "FoBaT"
        String[] queries3 = {"FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"};
        String pattern3 = "FoBaT";
        List<Boolean> result3 = camelMatch(queries3, pattern3);
        System.out.println("Test case 3 result: " + result3);
    }
}

```





