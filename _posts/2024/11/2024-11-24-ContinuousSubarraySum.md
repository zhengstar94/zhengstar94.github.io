---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "523. Continuous Subarray Sum"
date: "2024-11-24"
categories:
  - "LeetCode HashTable"
---

- Given an integer array nums and an integer k, return `true` *if* `nums` *has a **good subarray** or* `false` *otherwise*.
- A **good subarray** is a subarray where:
  - its length is **at least two**, and
  - the sum of the elements of the subarray is a multiple of `k`.
- **Note** that:
  - A **subarray** is a contiguous part of the array.
  - An integer `x` is a multiple of `k` if there exists an integer `n` such that `x = n * k`. `0` is **always** a multiple of `k`.

**Example 1**

```
Input: nums = [23,2,4,6,7], k = 6
Output: true
Explanation: [2, 4] is a continuous subarray of size 2 whose elements sum up to 6.
```

**Example 2**

```
Input: nums = [23,2,6,4,7], k = 6
Output: true
Explanation: [23, 2, 6, 4, 7] is an continuous subarray of size 5 whose elements sum up to 42.
42 is a multiple of 6 because 42 = 7 * 6 and 7 is an integer.
```

**Example 3**

```
Input: nums = [23,2,6,4,7], k = 13
Output: false
```

## Method 1

```tex
【O(n) time | O(min(n, k)) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2024/11/24
 */
public class ContinuousSubarraySum {

    public static boolean checkSubarraySum(int[] nums, int k) {
        // HashMap to store remainder as key and index as value
        // Key: running sum % k, Value: index where this remainder was first seen
        Map<Integer, Integer> preSumMap = new HashMap<>();

        // Initialize map with 0 remainder at index -1
        // This handles the case where the subarray starts from index 0
        preSumMap.put(0, -1);

        // Running sum variable to keep track of cumulative sum % k
        int curSum = 0;

        // Iterate through the array
        for (int i = 0; i < nums.length; i++) {
            // Update running sum and take modulo to keep remainder
            // Using property: (a + b) % k = ((a % k) + (b % k)) % k
            curSum = (curSum + nums[i]) % k;

            // If we've seen this remainder before
            if (preSumMap.containsKey(curSum)) {
                // Check if the subarray length is at least 2
                // Current index - Previous index where we saw the same remainder
                if (i - preSumMap.get(curSum) >= 2) {
                    // Found a valid subarray whose sum is divisible by k
                    return true;
                }
            } else {
                // First time seeing this remainder, store it with current index
                preSumMap.put(curSum, i);
            }
        }

        // No valid subarray found
        return false;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Expected output: true
        // Subarray [2,4] has sum 6, which is divisible by 6
        int[] nums1 = {23, 2, 4, 6, 7};
        int k1 = 6;
        System.out.println("Test case 1: " + checkSubarraySum(nums1, k1));

        // Test Case 2: Expected output: true
        // The entire array sum is 42, which is divisible by 6
        int[] nums2 = {23, 2, 6, 4, 7};
        int k2 = 6;
        System.out.println("Test case 2: " + checkSubarraySum(nums2, k2));

        // Test Case 3: Expected output: false
        // No subarray with sum divisible by 13
        int[] nums3 = {23, 2, 6, 4, 7};
        int k3 = 13;
        System.out.println("Test case 3: " + checkSubarraySum(nums3, k3));

        // Test Case 4: Edge case - single element array
        // Expected output: false (subarray must have length >= 2)
        int[] nums4 = {1};
        int k4 = 1;
        System.out.println("Test case 4: " + checkSubarraySum(nums4, k4));

        // Test Case 5: Expected output: true
        // Contains subarray with sum divisible by 7
        int[] nums5 = {23, 2, 4, 6, 6};
        int k5 = 7;
        System.out.println("Test case 5: " + checkSubarraySum(nums5, k5));
    }
}
```





