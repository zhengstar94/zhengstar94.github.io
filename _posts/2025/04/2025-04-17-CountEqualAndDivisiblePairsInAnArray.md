---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2176. Count Equal and Divisible Pairs in an Array"
date: "2025-04-17"
tags: Easy
categories:
  - "LeetCode HashTable"
---


- Given a **0-indexed** integer array `nums` of length `n` and an integer `k`, return *the **number of pairs*** `(i, j)` *where* `0 <= i < j < n`, *such that* `nums[i] == nums[j]` *and* `(i * j)` *is divisible by* `k`.

**Example 1**

```
Input: nums = [3,1,2,2,2,1,3], k = 2
Output: 4
Explanation:
There are 4 pairs that meet all the requirements:
- nums[0] == nums[6], and 0 * 6 == 0, which is divisible by 2.
- nums[2] == nums[3], and 2 * 3 == 6, which is divisible by 2.
- nums[2] == nums[4], and 2 * 4 == 8, which is divisible by 2.
- nums[3] == nums[4], and 3 * 4 == 12, which is divisible by 2.
```

**Example 2**

```
Input: nums = [1,2,3,4], k = 1
Output: 0
Explanation: Since no value in nums is repeated, there are no pairs (i,j) that meet all the requirements.
```

## Method 1

```tex
【O(n²) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2025/04/17
 */
public class CountEqualAndDivisiblePairsInAnArray {
    
    public static int countPairs(int[] nums, int k) {
        // HashMap to store indices for each number
        // Key: number from array
        // Value: list of indices where this number appears
        Map<Integer, List<Integer>> map = new HashMap<>();
        int count = 0;

        // Iterate through the array to find valid pairs
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(nums[i])) {
                // Check all previous indices of the current number
                // to find valid pairs
                for (int prevIndex : map.get(nums[i])) {
                    // Use long to prevent integer overflow
                    // Check if product of indices is divisible by k
                    if ((long) i * prevIndex % k == 0) {
                        count++;
                    }
                }
            } else {
                // Initialize new ArrayList for first occurrence of a number
                map.put(nums[i], new ArrayList<>());
            }
            // Add current index to the list of indices for this number
            map.get(nums[i]).add(i);
        }

        return count;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Multiple valid pairs
        // Expected output: 4
        int[] nums1 = {3,1,2,2,2,1,3};
        int k1 = 2;
        System.out.println("Test Case 1 Result: " + countPairs(nums1, k1));

        // Test Case 2: No valid pairs
        // Expected output: 0
        int[] nums2 = {1,2,3,4};
        int k2 = 1;
        System.out.println("Test Case 2 Result: " + countPairs(nums2, k2));
    }
}

```





