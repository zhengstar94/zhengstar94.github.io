---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2099. Find Subsequence of Length K With the Largest Sum"
date: "2025-06-28"
tags: Easy
categories:
  - "LeetCode Array"
---


- You are given an integer array `nums` and an integer `k`. You want to find a **subsequence** of `nums` of length `k` that has the **largest** sum.
- Return ***any** such subsequence as an integer array of length* `k`.
- A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1**

```
Input: nums = [2,1,3,3], k = 2
Output: [3,3]
Explanation:
The subsequence has the largest sum of 3 + 3 = 6.
```

**Example 2**

```
Input: nums = [-1,-2,3,4], k = 3
Output: [-1,3,4]
Explanation: 
The subsequence has the largest sum of -1 + 3 + 4 = 6.
```

**Example 3**

```
Input: nums = [3,4,3,3], k = 2
Output: [3,4]
Explanation:
The subsequence has the largest sum of 3 + 4 = 7. 
Another possible subsequence is [4, 3].
```

## Method 1

```tex
【O(nlog(n)) time | O(n) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;
import java.util.Comparator;

/**
 * @Author zhengxingxing
 * @Date 2025/06/28
 */
public class FindSubsequenceOfLengthKWithTheLargestSum {
    
    public static int[] maxSubsequence(int[] nums, int k) {
        // Create a 2D array to store pairs of values and their original indices.
        // The first column will hold the values from nums, and the second column will hold their indices.
        int[][] pairs = new int[nums.length][2];

        // Populate the pairs array with values and their corresponding indices.
        for (int i = 0; i < nums.length; i++) {
            pairs[i][0] = nums[i]; // Store the value from nums
            pairs[i][1] = i;       // Store the original index of the value
        }

        // Sort the pairs array based on the values in descending order.
        // This allows us to easily access the largest values.
        Arrays.sort(pairs, (a, b) -> b[0] - a[0]);

        // Extract the top k elements from the sorted pairs array.
        // These elements will be the largest k values along with their indices.
        int[][] topK = Arrays.copyOfRange(pairs, 0, k);

        // Sort the top k elements based on their original indices to maintain the order in the original array.
        Arrays.sort(topK, Comparator.comparingInt(a -> a[1]));

        // Create an array to hold the result subsequence.
        int[] result = new int[k];

        // Populate the result array with the values from the top k elements.
        for (int i = 0; i < k; i++) {
            result[i] = topK[i][0]; // Extract the value from the sorted top k elements
        }

        // Return the resulting subsequence.
        return result;
    }

    // Main method to test the maxSubsequence function with various test cases.
    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {2, 1, 3, 3};
        int k1 = 2;
        // Expected output: [3, 3]
        System.out.println(Arrays.toString(maxSubsequence(nums1, k1)));

        // Test case 2
        int[] nums2 = {-1, -2, 3, 4};
        int k2 = 3;
        // Expected output: [-1, 3, 4]
        System.out.println(Arrays.toString(maxSubsequence(nums2, k2)));

        // Test case 3
        int[] nums3 = {3, 4, 3, 3};
        int k3 = 2;
        // Expected output: [3, 4]
        System.out.println(Arrays.toString(maxSubsequence(nums3, k3)));
    }
}

```





