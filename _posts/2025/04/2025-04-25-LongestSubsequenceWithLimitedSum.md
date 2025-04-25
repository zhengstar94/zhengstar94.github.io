---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2389. Longest Subsequence With Limited Sum"
date: "2025-04-25"
tags: Easy
categories:
  - "LeetCode Binary Search"
---


- You are given an integer array `nums` of length `n`, and an integer array `queries` of length `m`.
- Return *an array* `answer` *of length* `m` *where* `answer[i]` *is the **maximum** size of a **subsequence** that you can take from* `nums` *such that the **sum** of its elements is less than or equal to* `queries[i]`.
- A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1**

```
Input: nums = [4,5,2,1], queries = [3,10,21]
Output: [2,3,4]
Explanation: We answer the queries as follows:
- The subsequence [2,1] has a sum less than or equal to 3. It can be proven that 2 is the maximum size of such a subsequence, so answer[0] = 2.
- The subsequence [4,5,1] has a sum less than or equal to 10. It can be proven that 3 is the maximum size of such a subsequence, so answer[1] = 3.
- The subsequence [4,5,2,1] has a sum less than or equal to 21. It can be proven that 4 is the maximum size of such a subsequence, so answer[2] = 4.
```

**Example 2**

```
Input: nums = [2,3,4,5], queries = [1]
Output: [0]
Explanation: The empty subsequence is the only subsequence that has a sum less than or equal to 1, so answer[0] = 0.
```

## Method 1

```tex
【O(nlogn) time | O(n) space】
```

```java
package Leetcode.BinarySearch;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/04/25
 */
public class LongestSubsequenceWithLimitedSum {
    
    public static int[] answerQueries(int[] nums, int[] queries) {
        // Sort array first since we only care about sum, not order
        Arrays.sort(nums);
        int n = nums.length;
        int m = queries.length;

        // Create prefix sum array to store cumulative sums
        // preSum[i] represents sum of first (i+1) smallest numbers
        int[] preSum = new int[n];
        preSum[0] = nums[0];  // First element is same as nums[0]

        // Calculate prefix sum for rest of the array
        // preSum[i] = sum of all elements from index 0 to i
        for (int i = 1; i < n; i++) {
            preSum[i] = preSum[i - 1] + nums[i];
        }

        // Process each query using binary search
        int[] answer = new int[m];
        for (int i = 0; i < m; i++) {
            // For each query, find the longest subsequence with sum <= query value
            answer[i] = binarySearch(preSum, queries[i]);
        }

        return answer;
    }
    
    private static int binarySearch(int[] preSum, int target) {
        int left = 0;
        int right = preSum.length;

        while (left < right) {
            // Calculate middle point avoiding overflow
            int mid = left + (right - left) / 2;

            if (preSum[mid] > target) {
                // If current sum is too large, look in left half
                right = mid;
            } else {
                // If current sum is <= target, look in right half
                // We add 1 to left because we know mid is valid
                left = mid + 1;
            }
        }
        // 'left' will be the position of first element > target
        // This means we can use 'left' numbers in our subsequence
        return left;
    }

    public static void main(String[] args) {
        // Test case 1: Expected output [2,3,4]
        int[] nums1 = {4,5,2,1};
        int[] queries1 = {3,10,21};
        System.out.println("Test case 1 result: " + Arrays.toString(answerQueries(nums1, queries1)));

        // Test case 2: Expected output [0]
        int[] nums2 = {2,3,4,5};
        int[] queries2 = {1};
        System.out.println("Test case 2 result: " + Arrays.toString(answerQueries(nums2, queries2)));
    }
}

```





