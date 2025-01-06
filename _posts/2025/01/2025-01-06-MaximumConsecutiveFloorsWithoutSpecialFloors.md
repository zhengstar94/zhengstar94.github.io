---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2274. Maximum Consecutive Floors Without Special Floors"
date: "2025-01-06"
tags: Medium
categories:
  - "LeetCode Array"
---

- Alice manages a company and has rented some floors of a building as office space. Alice has decided some of these floors should be **special floors**, used for relaxation only.
- You are given two integers `bottom` and `top`, which denote that Alice has rented all the floors from `bottom` to `top` (**inclusive**). You are also given the integer array `special`, where `special[i]` denotes a special floor that Alice has designated for relaxation.
- Return *the **maximum** number of consecutive floors without a special floor*.

**Example 1**

```
Input: bottom = 2, top = 9, special = [4,6]
Output: 3
Explanation: The following are the ranges (inclusive) of consecutive floors without a special floor:
- (2, 3) with a total amount of 2 floors.
- (5, 5) with a total amount of 1 floor.
- (7, 9) with a total amount of 3 floors.
Therefore, we return the maximum number which is 3 floors.
```

**Example 2**

```
Input: bottom = 6, top = 8, special = [7,6,8]
Output: 0
Explanation: Every floor rented is a special floor, so we return 0.
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/01/06
 */
public class MaximumConsecutiveFloorsWithoutSpecialFloors {
    public static int maxConsecutive(int bottom, int top, int[] special) {
        Arrays.sort(special);

        int maxLen = 0;

        // Check the first consecutive non-special floors (from bottom to first special floor)
        maxLen = Math.max(maxLen, special[0] - bottom);

        // Check consecutive non-special floors between adjacent special floors
        for (int i = 1; i < special.length; i++) {
            maxLen = Math.max(maxLen, special[i] - special[i-1] - 1);
        }

        // Check the last consecutive non-special floors (from last special floor to top)
        maxLen = Math.max(maxLen, top - special[special.length-1]);

        return maxLen;
    }

    public static void main(String[] args) {
        // Test case 1
        int bottom1 = 2, top1 = 9;
        int[] special1 = {4,6};
        System.out.println("Test case 1: " + maxConsecutive(bottom1, top1, special1));  // Expected output: 3

        // Test case 2
        int bottom2 = 6, top2 = 8;
        int[] special2 = {7,6,8};
        System.out.println("Test case 2: " + maxConsecutive(bottom2, top2, special2));  // Expected output: 0
    }
}
```





