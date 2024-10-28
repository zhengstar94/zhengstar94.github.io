---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "72.Edit Distance"
date: "2024-10-28"
categories:
  - "LeetCode Dynamic Programming"
---

- Given two strings `word1` and `word2`, return *the minimum number of operations required to convert `word1` to `word2`*.
- You have the following three operations permitted on a word:
  - Insert a character
  - Delete a character
  - Replace a character

**Example 1**

```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```

**Example 2**

```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

## Method 1

```tex
【O(m * n) time | O(m * n) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * @author zhengstars
 * @date 2024/10/28
 */
public class EditDistance {
    public static int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        int[][] dp = new int[m + 1][n + 1];

        // Initialize dp array for base cases
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i; // Minimum steps when word2 is empty (deletions only)
        }
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j; // Minimum steps when word1 is empty (insertions only)
        }

        // Fill the dp array using dynamic programming
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // If characters are equal, carry forward the value from dp[i-1][j-1]
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // Compute minimum operations by comparing:
                    // 1. Replacement (dp[i-1][j-1] + 1)
                    // 2. Insertion (dp[i][j-1] + 1)
                    // 3. Deletion (dp[i-1][j] + 1)
                    dp[i][j] = Math.min(dp[i - 1][j - 1],
                            Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
                }
            }
        }

        return dp[m][n]; // Final edit distance
    }

    public static void main(String[] args) {

        // Test case 1
        String word1 = "horse";
        String word2 = "ros";
        System.out.println("Output: " + minDistance(word1, word2));  // Expected: 3

        // Test case 2
        word1 = "intention";
        word2 = "execution";
        System.out.println("Output: " + minDistance(word1, word2));  // Expected: 5

        // Test case 3
        word1 = "";
        word2 = "abc";
        System.out.println("Output: " + minDistance(word1, word2));  // Expected: 3

        // Test case 4
        word1 = "abc";
        word2 = "";
        System.out.println("Output: " + minDistance(word1, word2));  // Expected: 3

        // Test case 5
        word1 = "abc";
        word2 = "abc";
        System.out.println("Output: " + minDistance(word1, word2));  // Expected: 0
    }
}
```





