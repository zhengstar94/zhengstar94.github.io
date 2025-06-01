---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2929. Distribute Candies Among Children II"
date: "2025-06-01"
tags: Medium
categories:
  - "LeetCode Math"
---

- You are given two positive integers `n` and `limit`.
- Return *the **total number** of ways to distribute* `n` *candies among* `3` *children such that no child gets more than* `limit` *candies.*

**Example 1**

```
Input: n = 5, limit = 2
Output: 3
Explanation: There are 3 ways to distribute 5 candies such that no child gets more than 2 candies: (1, 2, 2), (2, 1, 2) and (2, 2, 1).
```

**Example 2**

```
Input: n = 3, limit = 3
Output: 10
Explanation: There are 10 ways to distribute 3 candies such that no child gets more than 3 candies: (0, 0, 3), (0, 1, 2), (0, 2, 1), (0, 3, 0), (1, 0, 2), (1, 1, 1), (1, 2, 0), (2, 0, 1), (2, 1, 0) and (3, 0, 0).
```

## Method 1

```tex
【O(1) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @Author zhengxingxing
 * @Date 2025/06/01
 */
public class DistributeCandiesAmongChildrenII {
    
    public static long distributeCandies(int n, int limit) {
        // Using Inclusion-Exclusion Principle:
        // Valid solutions (≤ limit) = Total solutions - Invalid solutions (> limit)

        /*
         * Breaking down the formula: c2(n + 2) - 3 * c2(n - limit + 1) + 3 * c2(n - 2 * limit) - c2(n - 3 * limit - 1)
         *
         * Term 1: c2(n + 2)
         * - Total ways to distribute n candies among 3 children without any limit
         * - Using stars and bars method: place n candies in 3 distinguishable boxes
         * - We have n+2 positions, choose 2 positions for dividers
         * - Formula: C(n+2, 2)
         *
         * Term 2: -3 * c2(n - limit + 1)
         * - Subtract cases where at least one child gets more than limit candies
         * - Set A: child 1 gets > limit candies (we don't care about child 2 and 3)
         * - Set B: child 2 gets > limit candies
         * - Set C: child 3 gets > limit candies
         * - For Set A: child 1 gets at least (limit+1) candies
         *   Equivalent to: give child 1 exactly (limit+1) candies first,
         *   then distribute remaining (n-(limit+1)) candies among all 3 children
         * - Ways for Set A: C((n-(limit+1))+2, 2) = C(n-limit+1, 2)
         * - |A| = |B| = |C| = C(n-limit+1, 2), so total: 3 * C(n-limit+1, 2)
         *
         * Term 3: +3 * c2(n - 2 * limit)
         * - Add back cases where exactly two children get > limit candies (inclusion-exclusion)
         * - Set A∩B: both child 1 and child 2 get > limit candies
         * - Set A∩C: both child 1 and child 3 get > limit candies
         * - Set B∩C: both child 2 and child 3 get > limit candies
         * - For Set A∩B: both child 1 and child 2 get at least (limit+1) candies
         *   Give child 1 and child 2 each (limit+1) candies first,
         *   then distribute remaining (n-2*(limit+1)) candies among all 3 children
         * - Ways for Set A∩B: C((n-2*(limit+1))+2, 2) = C(n-2*limit, 2)
         * - |A∩B| = |A∩C| = |B∩C| = C(n-2*limit, 2), so total: 3 * C(n-2*limit, 2)
         *
         * Term 4: -c2(n - 3 * limit - 1)
         * - Subtract cases where all three children get > limit candies
         * - Set A∩B∩C: all three children get > limit candies
         * - Give each child (limit+1) candies first,
         *   then distribute remaining (n-3*(limit+1)) candies among all 3 children
         * - Ways: C((n-3*(limit+1))+2, 2) = C(n-3*limit-1, 2)
         *
         * Final formula by inclusion-exclusion principle:
         * |Valid| = |Total| - |A∪B∪C|
         * = |Total| - (|A|+|B|+|C| - |A∩B|-|A∩C|-|B∩C| + |A∩B∩C|)
         * = C(n+2,2) - 3*C(n-limit+1,2) + 3*C(n-2*limit,2) - C(n-3*limit-1,2)
         */

        return c2(n + 2)                    // Total ways without constraint  C（n+2，2）
                - 3 * c2(n - limit + 1)        // Subtract single violations  C（n-（limit+1）+2， 2）
                + 3 * c2(n - 2 * limit)        // Add back double violations  C(n-2*(limit+1)+2， 2)  
                - c2(n - 3 * limit - 1);       // Subtract triple violations  C(n-3*(limit+1)+2, 2)
    }


    private static long c2(int n) {
        return n > 1 ? (long) n * (n - 1) / 2 : 0;
    }

    public static void main(String[] args) {
        // Example 1: n=5, limit=2
        System.out.println("n=5, limit=2: " + distributeCandies(5, 2)); // Expected: 3

        // Example 2: n=3, limit=3  
        System.out.println("n=3, limit=3: " + distributeCandies(3, 3)); // Expected: 10
    }
}
```





