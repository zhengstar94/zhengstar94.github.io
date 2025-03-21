---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Interview Question 16.06. Smallest Difference"
date: "2025-03-21"
tags: Medium TwoPointers
categories:
  - "LeetCode DoubleSeqTwoPointers"
---

- Given two arrays of integers a and b, compute the pair of values (one value in each array) with the smallest non-negative difference. Return the difference.

**Example 1**

```
Input: a = {1, 3, 15, 11, 2}, b = {23, 127, 235, 19, 8} 
Output: 3 Explanation: The pair (11, 8) has the smallest difference of 3.
```

## Method 1

```tex
【O(nlogn + mlogm) time | O(1) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqTwoPointers;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/03/21
 */
public class SmallestDifference {

    public static int smallestDifference(int[] a, int[] b) {
        // Sort both arrays to enable two-pointer approach
        // Time complexity: O(nlogn + mlogm)
        Arrays.sort(a);
        Arrays.sort(b);

        // Initialize two pointers, one for each array
        int i = 0;  // pointer for array a
        int j = 0;  // pointer for array b

        // Use long to prevent integer overflow when calculating differences
        long minDiff = Long.MAX_VALUE;

        // Continue until we reach the end of either array
        while (i < a.length && j < b.length) {
            // Calculate absolute difference between current elements
            // Cast to long to prevent overflow during subtraction
            long diff = Math.abs((long)a[i] - (long)b[j]);

            // Update minimum difference if current difference is smaller
            minDiff = Math.min(minDiff, diff);

            // Move pointers based on comparison of current elements
            if (a[i] < b[j]) {
                // If current element in a is smaller, move pointer i
                // to try to get closer to b[j]
                i++;
            } else if (a[i] > b[j]) {
                // If current element in b is smaller, move pointer j
                // to try to get closer to a[i]
                j++;
            } else {
                // If elements are equal, we found the minimum possible difference (0)
                return 0;
            }
        }

        // Cast the result back to int as per problem requirement
        return (int)minDiff;
    }


    public static void main(String[] args) {
        // Test Case 1: Normal case with positive numbers
        int[] a1 = {1, 3, 15, 11, 2};
        int[] b1 = {23, 127, 235, 19, 8};
        System.out.println("Test Case 1 Result: " + smallestDifference(a1, b1));  // Expected: 3

        // Test Case 2: Arrays containing same number
        int[] a2 = {1, 2, 3, 4};
        int[] b2 = {2, 5, 6, 7};
        System.out.println("Test Case 2 Result: " + smallestDifference(a2, b2));  // Expected: 0

        // Test Case 3: Edge case with maximum integer values
        int[] a3 = {Integer.MAX_VALUE};
        int[] b3 = {Integer.MIN_VALUE};
        System.out.println("Test Case 3 Result: " + smallestDifference(a3, b3));

        // Test Case 4: Edge case with empty array
        int[] a4 = {};
        int[] b4 = {1};
        System.out.println("Test Case 4 Result: " + smallestDifference(a4, b4));
    }
}
```





