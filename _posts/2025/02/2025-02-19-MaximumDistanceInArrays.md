---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "624. Maximum Distance in Arrays"
date: "2025-02-19"
tags: Medium
categories:
  - "LeetCode Array"
---


- You are given `m` `arrays`, where each array is sorted in **ascending order**.
- You can pick up two integers from two different arrays (each array picks one) and calculate the distance. We define the distance between two integers `a` and `b` to be their absolute difference `|a - b|`.
- Return *the maximum distance*.

**Example 1**

```
Input: arrays = [[1,2,3],[4,5],[1,2,3]]
Output: 4
Explanation: One way to reach the maximum distance 4 is to pick 1 in the first or third array and pick 5 in the second array.
```

**Example 2**

```
Input: arrays = [[1],[1]]
Output: 0
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/02/19
 */
public class MaximumDistanceInArrays {
    public static int maxDistance(List<List<Integer>> arrays) {
        // Handle edge cases: if input is null or has less than 2 arrays
        if (arrays == null || arrays.size() < 2) {
            return 0;
        }

        // Initialize maximum distance found so far
        int maxDistance = 0;

        // Initialize minVal and maxVal with the first array's values
        // Since arrays are sorted, first element is minimum and last element is maximum
        int minVal = arrays.get(0).get(0);                      // Get minimum of first array
        int maxVal = arrays.get(0).get(arrays.get(0).size() - 1); // Get maximum of first array

        // Start from second array (index 1) since we've used first array for initialization
        for (int i = 1; i < arrays.size(); i++) {
            List<Integer> array = arrays.get(i);

            // Get current array's minimum (first element) and maximum (last element)
            int currentMin = array.get(0);                      // Current array's minimum
            int currentMax = array.get(array.size() - 1);       // Current array's maximum

            /* Key Part 1: Calculate maximum distance
             * We need to compare two possibilities:
             * 1. currentMax - minVal: Distance between current array's maximum and previous minimum
             *    Example: If previous minVal = 1 and currentMax = 5, distance = |5-1| = 4
             * 2. currentMin - maxVal: Distance between current array's minimum and previous maximum
             *    Example: If previous maxVal = 3 and currentMin = 4, distance = |4-3| = 1
             */
            maxDistance = Math.max(maxDistance, Math.abs(currentMax - minVal));
            maxDistance = Math.max(maxDistance, Math.abs(currentMin - maxVal));

            /* Key Part 2: Update global minimum and maximum values
             * After processing current array, update global min and max for next iterations
             * minVal: Keep track of the smallest value seen so far
             * maxVal: Keep track of the largest value seen so far
             *
             * Example:
             * If current state: minVal = 1, maxVal = 3
             * Current array: [4,5]
             * After update: minVal = min(1,4) = 1, maxVal = max(3,5) = 5
             */
            minVal = Math.min(minVal, currentMin);    // Update global minimum
            maxVal = Math.max(maxVal, currentMax);    // Update global maximum
        }

        return maxDistance;    // Return the maximum distance found
    }

    public static void main(String[] args) {
        // Test Case 1: Example with three arrays
        List<List<Integer>> arrays1 = new ArrayList<>();
        arrays1.add(Arrays.asList(1, 2, 3));     // First array
        arrays1.add(Arrays.asList(4, 5));        // Second array
        arrays1.add(Arrays.asList(1, 2, 3));     // Third array
        System.out.println("Input: " + arrays1 + " -> Output: " + maxDistance(arrays1)); // Expected: 4

        // Test Case 2: Minimum case with two arrays
        List<List<Integer>> arrays2 = new ArrayList<>();
        arrays2.add(Arrays.asList(1));           // First array
        arrays2.add(Arrays.asList(1));           // Second array
        System.out.println("Input: " + arrays2 + " -> Output: " + maxDistance(arrays2)); // Expected: 0

        // Test Case 3: Example with increasing values
        List<List<Integer>> arrays3 = new ArrayList<>();
        arrays3.add(Arrays.asList(1, 5));        // First array
        arrays3.add(Arrays.asList(4, 6, 8));     // Second array
        arrays3.add(Arrays.asList(10, 12));      // Third array
        System.out.println("Input: " + arrays3 + " -> Output: " + maxDistance(arrays3)); // Expected: 11
    }
}

```





