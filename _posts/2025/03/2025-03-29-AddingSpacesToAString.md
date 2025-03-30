---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2109. Adding Spaces to a String"
date: "2025-03-29"
tags: Medium TwoPointers
categories:
  - "LeetCode TwoPointers"
---


- You are given a **0-indexed** string `s` and a **0-indexed** integer array `spaces` that describes the indices in the original string where spaces will be added. Each space should be inserted **before** the character at the given index.
  - For example, given `s = "EnjoyYourCoffee"` and `spaces = [5, 9]`, we place spaces before `'Y'` and `'C'`, which are at indices `5` and `9` respectively. Thus, we obtain `"Enjoy **Y**our **C**offee"`.
- Return *the modified string **after** the spaces have been added.*

**Example 1**

```
Input: s = "LeetcodeHelpsMeLearn", spaces = [8,13,15]
Output: "Leetcode Helps Me Learn"
Explanation: 
The indices 8, 13, and 15 correspond to the underlined characters in "LeetcodeHelpsMeLearn".
We then place spaces before those characters.
```

**Example 2**

```
Input: s = "icodeinpython", spaces = [1,5,7,9]
Output: "i code in py thon"
Explanation:
The indices 1, 5, 7, and 9 correspond to the underlined characters in "icodeinpython".
We then place spaces before those characters.
```

**Example 3**

```
Input: s = "spacing", spaces = [0,1,2,3,4,5,6]
Output: " s p a c i n g"
Explanation:
We are also able to place spaces before the first character of the string.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2025/03/30
 */
public class AddingSpacesToAString {

    public static String addSpaces(String s, int[] spaces) {
        // Initialize StringBuilder with optimal capacity to avoid resizing
        StringBuilder ans = new StringBuilder(s.length() + spaces.length);

        // Counter for tracking the current position in spaces array
        int j = 0;

        // Iterate through each character in the input string
        for (int i = 0; i < s.length(); i++) {
            // Check if we need to add a space at current position
            if (j < spaces.length && spaces[j] == i) {
                ans.append(' ');
                j++; // Move to next space position
            }
            // Append the current character from input string
            ans.append(s.charAt(i));
        }

        // Convert StringBuilder to String and return
        return ans.toString();
    }


    public static void main(String[] args) {
        // Test Case 1: Adding spaces in a camel case string
        String s1 = "LeetcodeHelpsMeLearn";
        int[] spaces1 = {8,13,15};
        System.out.println("Test Case 1 Result: " + addSpaces(s1, spaces1));

        // Test Case 2: Adding spaces in a lowercase string
        String s2 = "icodeinpython";
        int[] spaces2 = {1,5,7,9};
        System.out.println("Test Case 2 Result: " + addSpaces(s2, spaces2));

        // Test Case 3: Adding spaces between every character
        String s3 = "spacing";
        int[] spaces3 = {0,1,2,3,4,5,6};
        System.out.println("Test Case 3 Result: " + addSpaces(s3, spaces3));
    }
}

```





