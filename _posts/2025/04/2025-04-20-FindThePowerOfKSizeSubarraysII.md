---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3255. Find the Power of K-Size Subarrays II"
date: "2025-04-20"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---


- You are given an array of integers `nums` of length `n` and a *positive* integer `k`.
- The **power** of an array is defined as:
  - Its **maximum** element if *all* of its elements are **consecutive** and **sorted** in **ascending** order.
  - -1 otherwise.
- You need to find the **power** of all subarrays of `nums` of size `k`.
- Return an integer array `results` of size `n - k + 1`, where `results[i]` is the *power* of `nums[i..(i + k - 1)]`.

**Example 1**

```
Input: nums = [1,2,3,4,3,2,5], k = 3

Output: [3,4,-1,-1,-1]

Explanation:

There are 5 subarrays of nums of size 3:
	[1, 2, 3] with the maximum element 3.
	[2, 3, 4] with the maximum element 4.
	[3, 4, 3] whose elements are not consecutive.
	[4, 3, 2] whose elements are not sorted.
	[3, 2, 5] whose elements are not consecutive.
```

**Example 2**

```
Input: nums = [2,2,2,2,2], k = 4

Output: [-1,-1]
```

**Example 3**

```
Input: nums = [3,2,3,2,3,2], k = 2

Output: [-1,3,-1,3,-1]
```

## Method 1

```tex
【O(n) time | O(n - k + 1) space】
```

```java
package Leetcode.GroupedLoop;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/04/20
 */
public class FindThePowerOfKSizeSubarraysII {

    public static int[] resultsArray(int[] nums, int k) {
        // Get the length of input array
        int n = nums.length;
        // Initialize result array with length n-k+1 (total possible subarrays of size k)
        int[] ans = new int[n - k + 1];
        // Fill result array with -1 as default value (indicating invalid subarrays)
        Arrays.fill(ans, -1);

        // Index to traverse through the array
        int i = 0;

        // Main loop to process the entire array
        while (i < n) {
            // Mark the start of a potential consecutive sequence
            int start = i;

            // Find the longest consecutive increasing sequence starting from index i
            // For example: in [1,2,3,4,3], starting from 1, this will find [1,2,3,4]
            while (i + 1 < n && nums[i] + 1 == nums[i + 1]) {
                i++;
            }

            // Calculate length of current consecutive sequence
            // len = end position (i) - start position + 1
            int len = i - start + 1;

            // If the consecutive sequence length is >= k, process all valid k-sized subarrays
            if (len >= k) {
                // Process each possible k-sized subarray within the consecutive sequence
                // For example: if sequence is [1,2,3,4] and k=3:
                // First iteration (j=0): processes [1,2,3]
                // Second iteration (j=1): processes [2,3,4]
                for (int j = start; j + k - 1 <= i; j++) {
                    // j is the start of current k-sized subarray
                    // j + k - 1 is the end of current k-sized subarray
                    // Store the last element of the k-sized subarray as its energy value
                    // For [1,2,3], store 3 at ans[0]
                    // For [2,3,4], store 4 at ans[1]
                    ans[j] = nums[j + k - 1];
                }
            }

            // Move to next position to start checking for new consecutive sequence
            i++;
        }

        return ans;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Array with consecutive increasing sequence
        int[] nums1 = {1, 2, 3, 4, 3, 2, 5};
        int k1 = 3;
        System.out.println("Test Case 1:");
        System.out.println("Input: " + Arrays.toString(nums1) + ", k = " + k1);
        System.out.println("Output: " + Arrays.toString(resultsArray(nums1, k1)));
        // Expected output: [3, 4, -1, -1, -1]

        // Test Case 2: Array with all same elements
        int[] nums2 = {2, 2, 2, 2, 2};
        int k2 = 4;
        System.out.println("\nTest Case 2:");
        System.out.println("Input: " + Arrays.toString(nums2) + ", k = " + k2);
        System.out.println("Output: " + Arrays.toString(resultsArray(nums2, k2)));
        // Expected output: [-1, -1]

        // Test Case 3: Array with alternating numbers
        int[] nums3 = {3, 2, 3, 2, 3, 2};
        int k3 = 2;
        System.out.println("\nTest Case 3:");
        System.out.println("Input: " + Arrays.toString(nums3) + ", k = " + k3);
        System.out.println("Output: " + Arrays.toString(resultsArray(nums3, k3)));
        // Expected output: [-1, -1, -1, -1, -1]
    }
}

```





