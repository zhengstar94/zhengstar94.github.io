---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3223. Minimum Length of String After Operations"
date: "2025-01-13"
tags: Medium
categories:
  - "LeetCode Math"
---


- You are given a string `s`.
- You can perform the following process on `s` **any** number of times:
  - Choose an index `i` in the string such that there is **at least** one character to the left of index `i` that is equal to `s[i]`, and **at least** one character to the right that is also equal to `s[i]`.
  - Delete the **closest** character to the **left** of index `i` that is equal to `s[i]`.
  - Delete the **closest** character to the **right** of index `i` that is equal to `s[i]`.
- Return the **minimum** length of the final string `s` that you can achieve.

**Example 1**

```
Input: s = "abaacbcbb"

Output: 5

Explanation:
We do the following operations:

Choose index 2, then remove the characters at indices 0 and 3. The resulting string is s = "bacbcbb".
Choose index 3, then remove the characters at indices 0 and 5. The resulting string is s = "acbcb".
```

**Example 2**

```
Input: s = "aa"

Output: 2

Explanation:
We cannot perform any operations, so we return the length of the original string.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/01/13
 */
public class MinimumLengthOfStringAfterOperations {
    public static int minimumLength(String s) {
        // Array to count frequency of each lowercase letter (a-z)
        int[] cnt = new int[26];

        // Count the frequency of each character in the string
        for (char ch: s.toCharArray()) {
            cnt[ch - 'a']++;  // Convert char to index (e.g., 'a'->0, 'b'->1, etc.)
        }

        int ans = 0;
        // Process each character's frequency to determine its contribution to final length
        for (int count: cnt) {
            if (count > 0) {
                /* Mathematical formula explanation:
                 * (count - 1) % 2 + 1 determines how many characters will remain:
                 *
                 * For count = 1: (1-1)%2 + 1 = 0%2 + 1 = 1  (one char remains)
                 * For count = 2: (2-1)%2 + 1 = 1%2 + 1 = 2  (two chars remain)
                 * For count = 3: (3-1)%2 + 1 = 2%2 + 1 = 1  (one char remains)
                 * For count = 4: (4-1)%2 + 1 = 3%2 + 1 = 2  (two chars remain)
                 *
                 * Pattern:
                 * - Odd counts (1,3,5,...)  -> 1 char remains
                 * - Even counts (2,4,6,...) -> 2 chars remain
                 *
                 * This works because:
                 * 1. We can delete 3 chars at a time
                 * 2. For odd counts, we can delete all but one char
                 * 3. For even counts, we must leave two chars
                 */
                ans += (count - 1) % 2 + 1;
            }
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test case 1
        String s1 = "abaacbcbb";
        System.out.println("Test case 1: " + s1);
        System.out.println("Expected output: 5");
        System.out.println("Actual output: " + minimumLength(s1));

        // Test case 2
        String s2 = "aa";
        System.out.println("\nTest case 2: " + s2);
        System.out.println("Expected output: 2");
        System.out.println("Actual output: " + minimumLength(s2));

        // Test case 3
        String s3 = "aaaaa";
        System.out.println("\nTest case 3: " + s3);
        System.out.println("Expected output: 1");
        System.out.println("Actual output: " + minimumLength(s3));
    }
}

```





