---
toc:
beginning: true
giscus_comments: true
layout: post
title: "1814. Count Nice Pairs in an Array"
date: "2025-07-30"
tags: Medium
categories:
    - "LeetCode HashTable"
---


- You are given an array `nums` that consists of non-negative integers. Let us define `rev(x)` as the reverse of the non-negative integer `x`. For example, `rev(123) = 321`, and `rev(120) = 21`. A pair of indices `(i, j)` is **nice** if it satisfies all of the following conditions:
    - `0 <= i < j < nums.length`
    - `nums[i] + rev(nums[j]) == nums[j] + rev(nums[i])`
- Return *the number of nice pairs of indices*. Since that number can be too large, return it **modulo** `10^9 + 7`.

**Example 1**

```
Input: nums = [42,11,1,97]
Output: 2
Explanation: The two pairs are:
 - (0,3) : 42 + rev(97) = 42 + 79 = 121, 97 + rev(42) = 97 + 24 = 121.
 - (1,2) : 11 + rev(1) = 11 + 1 = 12, 1 + rev(11) = 1 + 11 = 12.
```

**Example 2**

```
Input: nums = [13,10,35,24,76]
Output: 4
```

## Method 1

```tex
【O(n * k) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;
import java.util.Map;

/**
 * @Author zhengxingxing
 * @Date 2025/07/30
 */
public class CountNicePairsInAnArray {
    // The modulo value as required by the problem to prevent integer overflow
    static final int MOD = 1_000_000_007;
    
    public static int countNicePairs(int[] nums) {
        // This map will store the frequency of each difference value (num - rev(num))
        Map<Integer, Integer> countMap = new HashMap<>();
        long ans = 0; // Use long to prevent overflow during summation

        // Iterate through each number in the array
        for (int num : nums) {
            // Calculate the difference between the number and its reversed value
            int diff = num - rev(num);

            // Get the current count of this diff value in the map.
            // If this diff has appeared before, it means for each previous occurrence,
            // we can form a new nice pair with the current number.
            int cnt = countMap.getOrDefault(diff, 0);

            // Add the count to the answer. Each previous same diff forms a nice pair with this num.
            ans = (ans + cnt) % MOD;

            // Update the map: increment the count of this diff value by 1
            countMap.put(diff, cnt + 1);
        }

        // Return the total number of nice pairs, cast to int as required
        return (int) ans;
    }

    /**
     * Helper function to reverse the digits of a non-negative integer.
     * For example, rev(123) = 321, rev(120) = 21.
     *
     * @param x The integer to reverse.
     * @return The reversed integer.
     */
    private static int rev(int x) {
        int res = 0;
        // Extract digits from right to left and build the reversed number
        while (x > 0) {
            res = res * 10 + x % 10; // Add the last digit of x to res
            x /= 10; // Remove the last digit from x
        }
        return res;
    }

    public static void main(String[] args) {
        int[] nums1 = {42, 11, 1, 97};
        int[] nums2 = {13, 10, 35, 24, 76};
        int[] nums3 = {1, 100, 10, 1000};

        System.out.println(countNicePairs(nums1)); // Output: 2
        System.out.println(countNicePairs(nums2)); // Output: 4
        System.out.println(countNicePairs(nums3)); // Output: 0
    }
}

```





