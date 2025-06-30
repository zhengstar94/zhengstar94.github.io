---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "594. Longest Harmonious Subsequence"
date: "2025-06-30"
tags: Easy
categories:
  - "LeetCode HashTable"
---

- We define a harmonious array as an array where the difference between its maximum value and its minimum value is **exactly** `1`.
- Given an integer array `nums`, return the length of its longest harmonious subsequence among all its possible subsequences.

**Example 1**

```
Input: nums = [1,3,2,2,5,2,3,7]

Output: 5

Explanation:

The longest harmonious subsequence is [3,2,2,2,3].
```

**Example 2**

```
Input: nums = [1,2,3,4]

Output: 2

Explanation:

The longest harmonious subsequences are [1,2], [2,3], and [3,4], all of which have a length of 2.
```

**Example 3**

```
Input: nums = [1,1,1,1]

Output: 0

Explanation:

No harmonic subsequence exists.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;
import java.util.Map;

/**
 * @Author zhengxingxing
 * @Date 2025/06/30
 */
public class LongestHarmoniousSubsequence {
    
    public static int findLHS(int[] nums) {
        // Create a HashMap to store the frequency (count) of each number in the array.
        Map<Integer, Integer> countMap = new HashMap<>();
        // Traverse the array and populate the countMap.
        for (int num : nums) {
            // For each number, increment its count in the map.
            // If the number is not present, getOrDefault returns 0.
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }

        int maxLen = 0; // Variable to keep track of the maximum length found.

        // Iterate through each unique number (key) in the map.
        for (int key : countMap.keySet()) {
            // Check if the map contains the adjacent number (key + 1).
            // Only pairs of numbers with a difference of exactly 1 can form a harmonious subsequence.
            if (countMap.containsKey(key + 1)) {
                // If both key and key+1 exist, calculate the total count of these two numbers.
                int len = countMap.get(key) + countMap.get(key + 1);
                // Update maxLen if this pair forms a longer harmonious subsequence.
                maxLen = Math.max(maxLen, len);
            }
        }
        // Return the length of the longest harmonious subsequence found.
        return maxLen;
    }

    public static void main(String[] args) {
        // Test case 1: Example from the problem description.
        int[] nums1 = {1, 3, 2, 2, 5, 2, 3, 7};
        // Expected output: 5 ([3,2,2,2,3])
        System.out.println(findLHS(nums1));

        // Test case 2: Consecutive numbers.
        int[] nums2 = {1, 2, 3, 4};
        // Expected output: 2 ([1,2], [2,3], or [3,4])
        System.out.println(findLHS(nums2));

        // Test case 3: All numbers are the same.
        int[] nums3 = {1, 1, 1, 1};
        // Expected output: 0 (no harmonious subsequence)
        System.out.println(findLHS(nums3));
    }
}
```





