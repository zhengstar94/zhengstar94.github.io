---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3085. Minimum Deletions to Make String K-Special"
date: "2025-06-21"
tags: Medium
categories:
  - "LeetCode Greedy"
---


- You are given a string `word` and an integer `k`.
- We consider `word` to be **k-special** if `|freq(word[i]) - freq(word[j])| <= k` for all indices `i` and `j` in the string.
- Here, `freq(x)` denotes the frequency of the character `x` in `word`, and `|y|` denotes the absolute value of `y`.
- Return *the **minimum** number of characters you need to delete to make* `word` ***k-special\***.

**Example 1**

```
Input: word = "aabcaba", k = 0

Output: 3

Explanation: We can make word 0-special by deleting 2 occurrences of "a" and 1 occurrence of "c". Therefore, word becomes equal to "baba" where freq('a') == freq('b') == 2.
```

**Example 2**

```
Input: word = "dabdcbdcdcd", k = 2

Output: 2

Explanation: We can make word 2-special by deleting 1 occurrence of "a" and 1 occurrence of "d". Therefore, word becomes equal to "bdcbdcdcd" where freq('b') == 2, freq('c') == 3, and freq('d') == 4.
```

**Example 3**

```
Input: word = "aaabaaa", k = 2

Output: 1

Explanation: We can make word 2-special by deleting 1 occurrence of "b". Therefore, word becomes equal to "aaaaaa" where each letter's frequency is now uniformly 6.
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
 * @Author zhengxingxing
 * @Date 2025/06/21
 */
public class MinimumDeletionsToMakeStringKSpecial {

    public static int minimumDeletions(String word, int k) {
        // Step 1: Count the frequency of each character in the string.
        int[] freq = new int[26];
        for (char c : word.toCharArray()) {
            freq[c - 'a']++;
        }

        // Step 2: Collect all non-zero frequencies into a list.
        // Only characters that appear in the string are relevant for further processing.
        List<Integer> freqList = new ArrayList<>();
        for (int f : freq) {
            if (f > 0) {
                freqList.add(f);
            }
        }

        // Step 3: Try every possible lower bound (lo) for the frequency interval [lo, lo + k].
        // The goal is to find the interval that requires the fewest deletions to fit all frequencies within it.
        int ans = Integer.MAX_VALUE; // Initialize the answer with a large value.
        for (int lo : freqList) {
            int cnt = 0; // This will count the number of deletions needed for the current interval.
            for (int v : freqList) {
                if (v < lo) {
                    // If the frequency is less than the lower bound, we must delete all occurrences of this character.
                    cnt += v;
                } else if (v > lo + k) {
                    // If the frequency is greater than the upper bound, we must delete the excess occurrences.
                    cnt += v - (lo + k);
                }
                // If v is within [lo, lo + k], no deletion is needed for this character.
            }
            // Update the answer if the current interval requires fewer deletions.
            ans = Math.min(ans, cnt);
        }
        // Step 4: Return the minimum deletions found.
        return ans;
    }

    public static void main(String[] args) {
        // Test cases to verify the solution.
        System.out.println(minimumDeletions("aabcaba", 0)); // Output: 3
        System.out.println(minimumDeletions("dabdcbdcdcd", 2)); // Output: 2
        System.out.println(minimumDeletions("aaabaaa", 2)); // Output: 1
        System.out.println(minimumDeletions("abc", 1)); // Output: 0
        System.out.println(minimumDeletions("aaaaa", 0)); // Output: 0
    }
}

```





