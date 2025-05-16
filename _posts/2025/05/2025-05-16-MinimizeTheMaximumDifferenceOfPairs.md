---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2616. Minimize the Maximum Difference of Pairs"
date: "2025-05-16"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.MinimizeMaximum"
---



- You are given a **0-indexed** integer array `nums` and an integer `p`. Find `p` pairs of indices of `nums` such that the **maximum** difference amongst all the pairs is **minimized**. Also, ensure no index appears more than once amongst the `p` pairs.
- Note that for a pair of elements at the index `i` and `j`, the difference of this pair is `|nums[i] - nums[j]|`, where `|x|` represents the **absolute** **value** of `x`.
- Return *the **minimum** **maximum** difference among all* `p` *pairs.* We define the maximum of an empty set to be zero.

**Example 1**

```
Input: nums = [10,1,2,7,1,3], p = 2
Output: 1
Explanation: The first pair is formed from the indices 1 and 4, and the second pair is formed from the indices 2 and 5. 
The maximum difference is max(|nums[1] - nums[4]|, |nums[2] - nums[5]|) = max(0, 1) = 1. Therefore, we return 1.
```

**Example 2**

```
Input: nums = [4,2,1,2], p = 1
Output: 0
Explanation: Let the indices 1 and 3 form a pair. The difference of that pair is |2 - 2| = 0, which is the minimum we can attain.
```

## Method 1

```tex
【O(n*log(n)+n*log(U)) time | O(1) space】
```

```java
package Leetcode.BinarySearch.MinimizeMaximum;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/05/16
 */
public class MinimizeTheMaximumDifferenceOfPairs {
    
    public static int minimizeMax(int[] nums, int p) {
        // If no pairs are needed, return 0 immediately.
        if (p == 0){
            return 0;
        }

        // Sort the array so we can easily find close numbers (small differences).
        Arrays.sort(nums);
        int n = nums.length;

        // Initialize binary search boundaries.
        // left is the smallest possible difference (0),
        // right is the largest possible difference (max - min).
        int left = 0;
        int right = nums[n - 1] - nums[0];

        // Perform binary search to find the minimal threshold
        // such that we can form at least p valid pairs.
        while (left < right){
            int mid = left + (right - left) / 2;

            // If it's possible to form p pairs with max difference ≤ mid,
            // try smaller values (search left half).
            if (canFormPairs(nums, mid, p)) {
                right = mid;
            } else {
                // Otherwise, try larger threshold (search right half).
                left = mid + 1;
            }
        }

        // After convergence, left is the smallest valid threshold.
        return left;
    }
    
    private static boolean canFormPairs(int[] nums, int threshold, int p) {
        int cnt = 0;
        for (int i = 0; i < nums.length - 1; i++) {
            // If adjacent elements form a valid pair under the threshold
            if (nums[i + 1] - nums[i] <= threshold) {
                cnt++;  // Count the valid pair
                i++;    // Skip the next element, as it's already paired
            }
            // If not valid, move to the next element to try a new pair
        }
        // Return true if we've formed at least p pairs
        return cnt >= p;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {10, 1, 2, 7, 1, 3};
        int p1 = 2;
        System.out.println("Test case 1 result: " + minimizeMax(nums1, p1)); // Expected output: 1

        // Test case 2
        int[] nums2 = {4, 2, 1, 2};
        int p2 = 1;
        System.out.println("Test case 2 result: " + minimizeMax(nums2, p2)); // Expected output: 0

        // Additional test case
        int[] nums3 = {3, 4, 2, 3, 2, 1, 2};
        int p3 = 3;
        System.out.println("Test case 3 result: " + minimizeMax(nums3, p3)); // Expected output: 1
    }
}

```





