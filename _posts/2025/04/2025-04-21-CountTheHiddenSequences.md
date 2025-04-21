---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2145. Count the Hidden Sequences"
date: "2025-04-21"
tags: Medium
categories:
  - "LeetCode Math"
---


- You are given a **0-indexed** array of `n` integers `differences`, which describes the **differences** between each pair of **consecutive** integers of a **hidden** sequence of length `(n + 1)`. More formally, call the hidden sequence `hidden`, then we have that `differences[i] = hidden[i + 1] - hidden[i]`.
- You are further given two integers `lower` and `upper` that describe the **inclusive** range of values `[lower, upper]` that the hidden sequence can contain.
  - For example, given `differences = [1, -3, 4]`, `lower = 1`, `upper = 6`, the hidden sequence is a sequence of length `4` whose elements are in between `1` and `6` (**inclusive**).
    - `[3, 4, 1, 5]` and `[4, 5, 2, 6]` are possible hidden sequences.
    - `[5, 6, 3, 7]` is not possible since it contains an element greater than `6`.
    - `[1, 2, 3, 4]` is not possible since the differences are not correct.
- Return *the number of **possible** hidden sequences there are.* If there are no possible sequences, return `0`.

**Example 1**

```
Input: differences = [1,-3,4], lower = 1, upper = 6
Output: 2
Explanation: The possible hidden sequences are:
- [3, 4, 1, 5]
- [4, 5, 2, 6]
Thus, we return 2.
```

**Example 2**

```
Input: differences = [3,-4,5,1,-2], lower = -4, upper = 5
Output: 4
Explanation: The possible hidden sequences are:
- [-3, 0, -4, 1, 2, 0]
- [-2, 1, -3, 2, 3, 1]
- [-1, 2, -2, 3, 4, 2]
- [0, 3, -1, 4, 5, 3]
Thus, we return 4.
```

**Example 3**

```
Input: differences = [4,-7,2], lower = 3, upper = 6
Output: 0
Explanation: There are no possible hidden sequences. Thus, we return 0.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/04/21
 */
public class CountTheHiddenSequences {
    
    public static long countArray(int[] differences, int lower, int upper) {
        // Initialize tracking variables
        int x = 0;  // Minimum cumulative difference
        int y = 0;  // Maximum cumulative difference
        int cur = 0;  // Current cumulative difference

        // Iterate through the differences array
        for (int d: differences) {
            // Update current cumulative difference
            cur += d;

            // Update minimum and maximum cumulative differences
            x = Math.min(x, cur);  // Track how far below start we go
            y = Math.max(y, cur);  // Track how far above start we go

            // If the required range (y-x) exceeds the allowed range (upper-lower),
            // it's impossible to construct a valid array
            if (y - x > upper - lower){
                return 0;
            }
        }

        // Calculate the number of possible starting values that would create valid arrays
        // (upper - lower): total allowed range
        // (y - x): minimum required range
        // +1: account for inclusive bounds
        return (upper - lower) - (y - x) + 1;
    }

    public static void main(String[] args) {
        // Test Case 1: Example with possible solutions
        int[] differences1 = {1, -3, 4};
        int lower1 = 1;
        int upper1 = 6;
        System.out.println("Test Case 1 Result: " + countArray(differences1, lower1, upper1)); // Expected: 2

        // Test Case 2: Example with more possible solutions
        int[] differences2 = {3, -4, 5, 1, -2};
        int lower2 = -4;
        int upper2 = 5;
        System.out.println("Test Case 2 Result: " + countArray(differences2, lower2, upper2)); // Expected: 4

        // Test Case 3: Example with no possible solutions
        int[] differences3 = {4, -7, 2};
        int lower3 = 3;
        int upper3 = 6;
        System.out.println("Test Case 3 Result: " + countArray(differences3, lower3, upper3)); // Expected: 0
    }
}

```





