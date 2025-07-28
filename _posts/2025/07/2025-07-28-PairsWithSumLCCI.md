---
toc:
beginning: true
giscus_comments: true
layout: post
title: "16.24. Pairs With Sum LCCI"
date: "2025-07-28"
tags: Medium
categories:
    - "LeetCode HashTable"
---

- Design an algorithm to find all pairs of integers within an array which sum to a specified value.

**Example 1**

```
Input: nums = [5,6,5], target = 11
Output: [ [ 5,6 ] ]
```

**Example 2**

```
Input: nums = [5,6,5,6], target = 11
Output: [ [ 5,6],[5,6 ] ]
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.*;

/**
 * @Author zhengxingxing
 * @Date 2025/07/28
 */
public class PairsWithSumLCCI {

    public static List<List<Integer>> pairSums(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        // This map records how many times each number appears in the array.
        Map<Integer, Integer> countMap = new HashMap<>();

        // Count the occurrences of each number in the array.
        for (int num: nums) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }

        // Iterate through the array to find valid pairs.
        for (int num: nums) {
            int complement = target - num; // The number needed to form the target sum with 'num'.

            // Check if both 'num' and its complement are still available (not used up in previous pairs).
            if (countMap.getOrDefault(num, 0) > 0 && countMap.getOrDefault(complement, 0) > 0) {
                // Special case: when num == complement, we need at least two of this number to form a pair.
                // For example, if target=10 and num=5, we need two 5's to form [5,5].
                if (num == complement && countMap.get(num) < 2) {
                    // If there is only one occurrence left, we cannot form a pair [num, num].
                    continue;
                }
                // Add the pair [num, complement] to the result list.
                result.add(Arrays.asList(num, complement));
                // Decrement the count for both numbers, since they have been used in a pair.
                countMap.put(num, countMap.get(num) - 1);
                countMap.put(complement, countMap.get(complement) - 1);
            }
            // If either number is not available, skip to the next.
        }
        return result;
    }

    public static void main(String[] args) {
        int[] nums1 = {5, 6, 5};
        int target1 = 11;
        // Output: [ [5, 6] ]
        System.out.println(pairSums(nums1, target1));

        int[] nums2 = {5, 6, 5, 6};
        int target2 = 11;
        // Output: [ [5, 6], [5, 6] ]
        System.out.println(pairSums(nums2, target2));

        int[] nums3 = {1, 2, 3, 4, 5};
        int target3 = 6;
        // Output: [ [1, 5], [2, 4] ]
        System.out.println(pairSums(nums3, target3));
    }
}

```





