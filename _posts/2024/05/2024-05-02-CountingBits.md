---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "338.Counting Bits"
date: "2024-05-02"
categories:
  - "LeetCode BitManipulation"
---
# 338. Counting Bits

- Given an integer `n`, return *an array* `ans` *of length* `n + 1` *such that for each* `i` (`0 <= i <= n`)*,* `ans[i]` *is the **number of*** `1`***'s** in the binary representation of* `i`.

**Example 1**

```
Input: n = 2
Output: [0,1,1]
Explanation:
0 --> 0
1 --> 1
2 --> 10
```

**Example 2**

```
Input: n = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.BitManipulation;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/06/09
 */
public class CountingBits {
    public static int[] countBits(int n) {
        // Initialize the result array with length n + 1.
        int[] ans = new int[n + 1];

        // Loop through the numbers from 1 to n.
        for (int i = 1; i <= n; i++) {
            // The number of 1's in the binary representation of i is equal to:
            // the number of 1's in i / 2 (i.e., i >> 1) plus
            // 1 if the least significant bit of i is 1 (i.e., i & 1).
            // This is based on the dynamic programming principle where the solution
            // for a larger problem is built from the solutions of smaller subproblems.
            ans[i] = ans[i >> 1] + (i & 1);
        }

        // Return the result array.
        return ans;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] result1 = countBits(2);
        System.out.println(Arrays.toString(result1)); // Expected: [0, 1, 1]

        // Test case 2
        int[] result2 = countBits(5);
        System.out.println(Arrays.toString(result2)); // Expected: [0, 1, 1, 2, 1, 2]
    }
}

```

