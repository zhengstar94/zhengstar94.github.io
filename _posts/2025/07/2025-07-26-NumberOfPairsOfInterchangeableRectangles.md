---
toc:
beginning: true
giscus_comments: true
layout: post
title: "2001. Number of Pairs of Interchangeable Rectangles"
date: "2025-07-26"
tags: Medium
categories:
    - "LeetCode HashTable"
---


- You are given `n` rectangles represented by a **0-indexed** 2D integer array `rectangles`, where `rectangles[i] = [widthi, heighti]` denotes the width and height of the `ith` rectangle.
- Two rectangles `i` and `j` (`i < j`) are considered **interchangeable** if they have the **same** width-to-height ratio. More formally, two rectangles are **interchangeable** if `widthi/heighti == widthj/heightj` (using decimal division, not integer division).
- Return *the **number** of pairs of **interchangeable** rectangles in* `rectangles`.

**Example 1**

```
Input: rectangles = [ [ 4,8],[3,6],[10,20],[15,30 ] ]
Output: 6
Explanation: The following are the interchangeable pairs of rectangles by index (0-indexed):
- Rectangle 0 with rectangle 1: 4/8 == 3/6.
- Rectangle 0 with rectangle 2: 4/8 == 10/20.
- Rectangle 0 with rectangle 3: 4/8 == 15/30.
- Rectangle 1 with rectangle 2: 3/6 == 10/20.
- Rectangle 1 with rectangle 3: 3/6 == 15/30.
- Rectangle 2 with rectangle 3: 10/20 == 15/30.
```

**Example 2**

```
Input: rectangles = [ [ 4,5],[7,8 ] ]
Output: 0
Explanation: There are no interchangeable pairs of rectangles.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;
import java.util.Map;

/**
 * @Author zhengxingxing
 * @Date 2025/07/26
 */
public class NumberOfPairsOfInterchangeableRectangles {

    public static long interchangeableRectangles(int[][] rectangles) {
        long ans = 0; // This variable will store the final answer: the total number of interchangeable pairs.

        // This HashMap will store, for each unique ratio, how many times it has appeared so far.
        // Key: the ratio (width / height) as a double.
        // Value: the count of rectangles seen so far with this ratio.
        Map<Double, Integer> map = new HashMap<>();

        // Iterate through each rectangle in the input array.
        for (int[] rect: rectangles) {
            // Calculate the ratio of width to height for the current rectangle.
            // Multiplying by 1.0 ensures the division is floating-point, not integer division.
            double ratio = rect[0] * 1.0 / rect[1];

            // For the current rectangle, check how many rectangles with the same ratio have already been seen.
            // Each such rectangle can form a unique pair with the current one.
            // So, add the count of rectangles with this ratio to the answer.
            ans += map.getOrDefault(ratio, 0);

            // Update the map: increment the count for this ratio by 1,
            // because we've now seen one more rectangle with this ratio.
            map.put(ratio, map.getOrDefault(ratio, 0) + 1);
        }

        // After processing all rectangles, return the total number of interchangeable pairs found.
        return ans;
    }

    public static void main(String[] args) {
        // Example 1: rectangles with the same ratio (4/8 = 3/6 = 10/20 = 15/30 = 0.5)
        int[][] rectangles1 = { { 4,8},{3,6},{10,20},{15,30 } };
        // Example 2: rectangles with different ratios (4/5 != 7/8)
        int[][] rectangles2 = { { 4,5},{7,8 } };

        // Output the results for both examples.
        // For rectangles1, the output should be 6 (all rectangles are interchangeable).
        System.out.println(interchangeableRectangles(rectangles1)); // Output: 6

        // For rectangles2, the output should be 0 (no interchangeable pairs).
        System.out.println(interchangeableRectangles(rectangles2)); // Output: 0
    }
}

```





