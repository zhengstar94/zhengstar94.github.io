---
toc:
beginning: true
giscus_comments: true
layout: post
title: "2441. Largest Positive Integer That Exists With Its Negative"
date: "2025-07-21"
tags: Easy
categories:
- "LeetCode HashTable"
---


- Given an integer array `nums` that **does not contain** any zeros, find **the largest positive** integer `k` such that `-k` also exists in the array.
- Return *the positive integer* `k`. If there is no such integer, return `-1`.

**Example 1**

```
Input: nums = [-1,2,-3,3]
Output: 3
Explanation: 3 is the only valid k we can find in the array.
```

**Example 2**

```
Input: nums = [-1,10,6,7,-7,1]
Output: 7
Explanation: Both 1 and 7 have their corresponding negative values in the array. 7 has a larger value.
```

**Example 3**

```
Input: nums = [-10,8,6,7,-2,-3]
Output: -1
Explanation: There is no a single valid k, we return -1.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.HashSet;

/**
 * @Author zhengxingxing
 * @Date 2025/07/21
 */
public class LargestPositiveIntegerThatExistsWithItsNegative {

    public static int findMaxK(int[] nums) {
        // Create a HashSet to store all numbers seen so far for quick lookup
        HashSet<Integer> set = new HashSet<>();
        // Initialize maxK to -1, which will be returned if no valid k is found
        int maxK = -1;

        // Iterate through each number in the input array
        for (int num : nums) {
            // Check if the opposite number (-num) has already been seen
            if (set.contains(-num)) {
                // If so, update maxK to the larger value between current maxK and |num|
                // Math.abs(num) ensures we always consider the positive value
                maxK = Math.max(maxK, Math.abs(num));
            }
            // Add the current number to the set for future lookups
            set.add(num);
        }

        // After checking all numbers, return the result
        return maxK;
    }

    public static void main(String[] args) {
        // Example 1: Both 3 and -3 exist, so the answer is 3
        int[] nums1 = {-1, 2, -3, 3};
        // Example 2: Both 7 and -7 exist, so the answer is 7
        int[] nums2 = {-1, 10, 6, 7, -7, 1};
        // Example 3: No number has its negative, so the answer is -1
        int[] nums3 = {-10, 8, 6, 7, -2, -3};

        // Print the results of each test case
        System.out.println(findMaxK(nums1)); // Output: 3
        System.out.println(findMaxK(nums2)); // Output: 7
        System.out.println(findMaxK(nums3)); // Output: -1
    }
}

```





