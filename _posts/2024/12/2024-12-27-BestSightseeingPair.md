---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1014. Best Sightseeing Pair"
date: "2024-12-27"
tags: Medium
categories:
  - "LeetCode Array"
---


- You are given an integer array `values` where values[i] represents the value of the `ith` sightseeing spot. Two sightseeing spots `i` and `j` have a **distance** `j - i` between them.
- The score of a pair (`i < j`) of sightseeing spots is `values[i] + values[j] + i - j`: the sum of the values of the sightseeing spots, minus the distance between them.
- Return *the maximum score of a pair of sightseeing spots*.

**Example 1**

```
Input: values = [8,1,5,2,6]
Output: 11
Explanation: i = 0, j = 2, values[i] + values[j] + i - j = 8 + 5 + 0 - 2 = 11
```

**Example 2**

```
Input: values = [1,2]
Output: 2
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2024/12/27
 */
public class BestSightseeingPair {
    public static int maxScoreSightseeingPair(int[] values) {
        // Keep track of maximum values[i] + i encountered so far
        int maxPrev = values[0] + 0;
        // Store the maximum score found
        int maxScore = 0;

        // Iterate through array starting from second element
        for (int j = 1; j < values.length; j++) {
            // Update maximum score using current position and previous maximum
            maxScore = Math.max(maxScore, maxPrev + values[j] - j);
            // Update the maximum values[i] + i for next iterations
            maxPrev = Math.max(maxPrev, values[j] + j);
        }

        return maxScore;
    }

    public static void main(String[] args) {
        // Test case 1: Expected output is 11 (i=0, j=2)
        int[] test1 = {8,1,5,2,6};
        System.out.println("Test case 1 expected: 11");
        System.out.println("Test case 1 actual: " + maxScoreSightseeingPair(test1));

        // Test case 2: Expected output is 2
        int[] test2 = {1,2};
        System.out.println("Test case 2 expected: 2");
        System.out.println("Test case 2 actual: " + maxScoreSightseeingPair(test2));

        // Additional test case
        int[] test3 = {1,2,3,4,5};
        System.out.println("Test case 3 result: " + maxScoreSightseeingPair(test3));
    }
}

```





