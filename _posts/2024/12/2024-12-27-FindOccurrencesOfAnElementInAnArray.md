---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3159. Find Occurrences of an Element in an Array"
date: "2024-12-27"
tags: Medium
categories:
  - "LeetCode Array"
---

- You are given an integer array `nums`, an integer array `queries`, and an integer `x`.
- For each `queries[i]`, you need to find the index of the `queries[i]th` occurrence of `x` in the `nums` array. If there are fewer than `queries[i]` occurrences of `x`, the answer should be -1 for that query.
- Return an integer array `answer` containing the answers to all queries.

**Example 1**

```
Input: nums = [1,3,1,7], queries = [1,3,2,4], x = 1

Output: [0,-1,2,-1]

Explanation:

For the 1st query, the first occurrence of 1 is at index 0.
For the 2nd query, there are only two occurrences of 1 in nums, so the answer is -1.
For the 3rd query, the second occurrence of 1 is at index 2.
For the 4th query, there are only two occurrences of 1 in nums, so the answer is -1.
```

**Example 2**

```
Input: nums = [1,2,3], queries = [10], x = 5

Output: [-1]

Explanation:

For the 1st query, 5 doesn't exist in nums, so the answer is -1.
```

## Method 1

```tex
【O(q+n) time | O(k) space】
```

```java
package Leetcode.Array;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2024/12/27
 */
public class FindOccurrencesOfAnElementInAnArray {
    public static int[] findIndices(int[] nums, int[] queries, int x) {
        // Store all positions where x appears in nums array
        List<Integer> positions = new ArrayList<>();

        // First pass: record all positions where x appears
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == x) {
                positions.add(i);
            }
        }

        // Create array to store answers for each query
        int[] answer = new int[queries.length];

        // Process each query
        for (int i = 0; i < queries.length; i++) {
            // Convert from 1-based counting (human-readable) to 0-based indexing (computer-readable)
            // Example:
            // If queries[i] = 1, we want first occurrence, so we need positions.get(0)
            // If queries[i] = 2, we want second occurrence, so we need positions.get(1)
            // Therefore, we subtract 1 to convert query number to array index
            int occurrence = queries[i] - 1;

            // Check if we have enough occurrences in our positions list
            if (occurrence >= positions.size()) {
                answer[i] = -1;  // Not enough occurrences found
            } else {
                answer[i] = positions.get(occurrence);  // Return the index of the requested occurrence
            }
        }

        return answer;
    }

    public static void main(String[] args) {
        // Test Case 1
        int[] nums1 = {1, 3, 1, 7};
        int[] queries1 = {1, 3, 2, 4};
        int x1 = 1;
        System.out.println("Test Case 1:");
        System.out.print("Input: nums = [1,3,1,7], queries = [1,3,2,4], x = 1");
        int[] result1 = findIndices(nums1, queries1, x1);
        System.out.println("\nOutput: " + Arrays.toString(result1));  // Expected: [0,-1,2,-1]

        // Test Case 2
        int[] nums2 = {1, 2, 3};
        int[] queries2 = {10};
        int x2 = 5;
        System.out.println("\nTest Case 2:");
        System.out.print("Input: nums = [1,2,3], queries = [10], x = 5");
        int[] result2 = findIndices(nums2, queries2, x2);
        System.out.println("\nOutput: " + Arrays.toString(result2));  // Expected: [-1]

        // Additional Test Case 3
        int[] nums3 = {1, 1, 1, 1};
        int[] queries3 = {1, 2, 3, 4, 5};
        int x3 = 1;
        System.out.println("\nTest Case 3:");
        System.out.print("Input: nums = [1,1,1,1], queries = [1,2,3,4,5], x = 1");
        int[] result3 = findIndices(nums3, queries3, x3);
        System.out.println("\nOutput: " + Arrays.toString(result3));  // Expected: [0,1,2,3,-1]
    }
}

```





