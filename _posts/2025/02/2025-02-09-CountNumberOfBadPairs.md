---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2364. Count Number of Bad Pairs"
date: "2025-02-09"
tags: Medium
categories:
  - "LeetCode Array"
---


- You are given a **0-indexed** integer array `nums`. A pair of indices `(i, j)` is a **bad pair** if `i < j` and `j - i != nums[j] - nums[i]`.
- Return *the total number of **bad pairs** in* `nums`.

**Example 1**

```
Input: nums = [4,1,3,3]
Output: 5
Explanation: The pair (0, 1) is a bad pair since 1 - 0 != 1 - 4.
The pair (0, 2) is a bad pair since 2 - 0 != 3 - 4, 2 != -1.
The pair (0, 3) is a bad pair since 3 - 0 != 3 - 4, 3 != -1.
The pair (1, 2) is a bad pair since 2 - 1 != 3 - 1, 1 != 2.
The pair (2, 3) is a bad pair since 3 - 2 != 3 - 3, 1 != 0.
There are a total of 5 bad pairs, so we return 5.
```

**Example 2**

```
Input: nums = [1,2,3,4,5]
Output: 0
Explanation: There are no bad pairs.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Array;

import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2025/02/09
 */
public class CountNumberOfBadPairs {

    public static long countBadPairs(int[] nums) {
        // Initialize a HashMap to store the frequency of nums[i] - i values.
        // This helps us efficiently count how many times a specific "difference" has occurred.
        Map<Integer, Integer> map = new HashMap<>();

        // Variable to count the number of good pairs, initialized to 0.
        // A good pair satisfies the condition nums[j] - j == nums[i] - i.
        long goodPairs = 0;

        // Iterate through the array, calculating the current difference and updating goodPairs.
        for (int i = 0; i < nums.length; i++) {
            // Calculate the difference (nums[i] - i), which is used to identify good pairs.
            int diff = nums[i] - i;

            // Add the number of previous occurrences of the current difference to goodPairs.
            // This indicates how many indices before i have the same difference, forming good pairs with the current index.
            goodPairs += map.getOrDefault(diff, 0);

            // Update the frequency of the current difference in the map.
            // This prepares for future iterations to count good pairs for subsequent elements.
            map.put(diff, map.getOrDefault(diff, 0) + 1);
        }

        // Calculate the total number of possible pairs in the array.
        // Using the formula for combinations: n * (n - 1) / 2 (where n = nums.length).
        long totalPairs = (long) nums.length * (nums.length - 1) / 2;

        // The number of bad pairs is equal to the total pairs minus the number of good pairs.
        return totalPairs - goodPairs;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Example array with some bad pairs
        int[] nums1 = {4, 1, 3, 3};
        System.out.println("Test Case 1 Result: " + countBadPairs(nums1)); // Expected output: 5

        // Test Case 2: Example array with no bad pairs (fully good pairs)
        int[] nums2 = {1, 2, 3, 4, 5};
        System.out.println("Test Case 2 Result: " + countBadPairs(nums2)); // Expected output: 0

        // Test Case 3: A large array with perfectly ordered elements (no bad pairs)
        int[] nums3 = new int[100000];
        for (int i = 0; i < nums3.length; i++) {
            nums3[i] = i + 1; // Each element is its index + 1
        }
        System.out.println("Test Case 3 Result: " + countBadPairs(nums3)); // Expected output: 0

        // Test Case 4: Array where all elements are identical
        int[] nums4 = {1, 1, 1, 1};
        System.out.println("Test Case 4 Result: " + countBadPairs(nums4)); // Expected output: 6
    }
}

```





