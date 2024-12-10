---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2981. Find Longest Special Substring That Occurs Thrice I"
date: "2024-12-10"
tags: Medium
categories:
  - "LeetCode String"
---

- You are given a string `s` that consists of lowercase English letters.
- A string is called **special** if it is made up of only a single character. For example, the string `"abc"` is not special, whereas the strings `"ddd"`, `"zz"`, and `"f"` are special.
- Return *the length of the **longest special substring** of* `s` *which occurs **at least thrice***, *or* `-1` *if no special substring occurs at least thrice*.
- A **substring** is a contiguous **non-empty** sequence of characters within a string.

**Example 1**

```
Input: s = "aaaa"
Output: 2
Explanation: The longest special substring which occurs thrice is "aa": substrings "aaaa", "aaaa", and "aaaa".
It can be shown that the maximum length achievable is 2.
```

**Example 2**

```
Input: s = "abcdef"
Output: -1
Explanation: There exists no special substring which occurs at least thrice. Hence return -1.
```

**Example 3**

```
Input: s = "abcaba"
Output: 1
Explanation: The longest special substring which occurs thrice is "a": substrings "abcaba", "abcaba", and "abcaba".
It can be shown that the maximum length achievable is 1.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.String;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2024/12/10
 */
public class FindLongestSpecialSubstringThatOccursThriceI {
    public static int maximumLength(String s) {
        // Convert the input string to a character array for easier manipulation in the next steps
        char[] tempArray = s.toCharArray();

        // Create an array of lists to store the lengths of contiguous substrings for each character (26 letters in the alphabet)
        List<Integer>[] groups = new ArrayList[26];
        Arrays.setAll(groups, i -> new ArrayList<>());

        // Variable to count the length of the current contiguous substring
        int cnt = 0;

        // Iterate over the character array to group contiguous occurrences of each character
        for (int i = 0; i < tempArray.length; i++) {
            cnt++; // Increment the current substring length
            // If we reach the end of the string or the current character is different from the next
            if(i + 1 == tempArray.length || tempArray[i] != tempArray[i + 1]){
                // Add the length of the current contiguous substring to the appropriate group
                groups[tempArray[i] - 'a'].add(cnt);
                cnt = 0; // Reset the counter for the next substring
            }
        }

        // Variable to store the overall maximum length of a special substring
        int ans = 0;

        // Iterate over each group (i.e., each letter's contiguous substring lengths)
        for (List<Integer> group: groups) {
            // Skip empty groups (i.e., letters that don't appear in the string)
            if (group.isEmpty()){
                continue;
            }
            // Add two zeroes to simulate the case where there are less than 3 substrings
            group.add(0); // Adding zero as a placeholder
            group.add(0); // Adding another zero as a placeholder

            // Calculate case 1: the longest substring minus 2 (this assumes the longest substring is the special one)
            int case1 = group.get(0) - 2;

            // Calculate case 2: the best possible substring when there are at least two substrings (find the minimum of the first two, then maximize it with the third)
            int case2 = Math.max(Math.min(group.get(0) - 1, group.get(1)), group.get(2));

            // Update the answer by comparing the current maximum with the newly calculated cases
            ans = Math.max(ans, Math.max(case1, case2));
        }

        // Return the result, or -1 if no valid special substring was found
        return ans > 0 ? ans : -1;
    }

    public static void main(String[] args) {
        // Test case 1: The string "aaabbcaaa" contains several groups of contiguous characters.
        String test1 = "aaabbcaaa";
        System.out.println("Test case 1: " + test1 + " -> Maximum length: " + maximumLength(test1));

        // Test case 2: The string "aabbaa" has a mix of substrings but not all substrings are of the same length.
        String test2 = "aabbaa";
        System.out.println("Test case 2: " + test2 + " -> Maximum length: " + maximumLength(test2));

        // Test case 3: The string "abc" contains no repeated substrings, so the maximum length will be 1.
        String test3 = "abc";
        System.out.println("Test case 3: " + test3 + " -> Maximum length: " + maximumLength(test3));

        // Test case 4: The string "aaaaa" has a single repeated character for a long length.
        String test4 = "aaaaa";
        System.out.println("Test case 4: " + test4 + " -> Maximum length: " + maximumLength(test4));

        // Test case 5: The string "a" has only one character, so the maximum length will be 1.
        String test5 = "a";
        System.out.println("Test case 5: " + test5 + " -> Maximum length: " + maximumLength(test5));
    }
}
```





