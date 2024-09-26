---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "139.Word Break"
date: "2024-09-26"
categories:
  - "LeetCode Dynamic Programming"
---

- Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.
- **Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1**

```
Input: s = "leetcode", wordDict = ["leet","code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example 2**

```
Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
```

**Example 3**

```
Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: false
```

## Method 1

```tex
【O(n^2) time | O(n + m) space】
```

```java
package Leetcode.DynamicProgramming;

import java.util.Arrays;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

/**
 * @author zhengstars
 * @date 2024/09/26
 */
public class WordBreak {
    public static boolean wordBreak(String s, List<String> wordDict) {
        // Convert wordDict to a HashSet for O(1) lookup time
        Set<String> wordSet = new HashSet<>(wordDict);

        // Create a boolean array to store the results of subproblems
        // dp[i] will be true if the substring s[0,i) can be segmented into words from the dictionary
        boolean[] dp = new boolean[s.length() + 1];

        // Base case: empty string is always valid
        dp[0] = true;

        // Iterate through all possible end positions of substrings
        for (int i = 1; i <= s.length(); i++) {
            // Try all possible starting positions for the last word
            for (int j = 0; j < i; j++) {
                // If the substring s[0,j) can be segmented (dp[j] is true)
                // and the substring s[j,i) is in the dictionary
                if (dp[j] && wordSet.contains(s.substring(j, i))) {
                    // Then the substring s[0,i) can be segmented
                    dp[i] = true;
                    // No need to check further for this i
                    break;
                }
            }
        }

        // Return whether the entire string can be segmented
        return dp[s.length()];
    }

    // Test cases
    public static void main(String[] args) {

        // Test case 1
        String s1 = "leetcode";
        List<String> wordDict1 = Arrays.asList("leet", "code");
        System.out.println("Test case 1: " + wordBreak(s1, wordDict1)); // Expected: true

        // Test case 2
        String s2 = "applepenapple";
        List<String> wordDict2 = Arrays.asList("apple", "pen");
        System.out.println("Test case 2: " + wordBreak(s2, wordDict2)); // Expected: true

        // Test case 3
        String s3 = "catsandog";
        List<String> wordDict3 = Arrays.asList("cats", "dog", "sand", "and", "cat");
        System.out.println("Test case 3: " + wordBreak(s3, wordDict3)); // Expected: false

        // Test case 4 (Empty string)
        String s4 = "";
        List<String> wordDict4 = Arrays.asList("a", "b", "c");
        System.out.println("Test case 4: " + wordBreak(s4, wordDict4)); // Expected: true

        // Test case 5 (Single character)
        String s5 = "a";
        List<String> wordDict5 = Arrays.asList("a");
        System.out.println("Test case 5: " + wordBreak(s5, wordDict5)); // Expected: true

        // Test case 6 (No valid segmentation)
        String s6 = "aaaaaaa";
        List<String> wordDict6 = Arrays.asList("aaaa", "aaaa");
        System.out.println("Test case 6: " + wordBreak(s6, wordDict6)); // Expected: false
    }
}

```



