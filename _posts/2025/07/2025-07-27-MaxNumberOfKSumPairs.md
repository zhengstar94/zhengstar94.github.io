---
toc:
beginning: true
giscus_comments: true
layout: post
title: "1679. Max Number of K-Sum Pairs"
date: "2025-07-27"
tags: Medium
categories:
    - "LeetCode HashTable"
---


- You are given an integer array `nums` and an integer `k`.
- In one operation, you can pick two numbers from the array whose sum equals `k` and remove them from the array.
- Return *the maximum number of operations you can perform on the array*.

**Example 1**

```
Input: nums = [112,131,411]

Output: -1

Explanation:

Each numbers largest digit in order is [2,3,4].
```

**Example 2**

```
Input: nums = [2536,1613,3366,162]

Output: 5902

Explanation:

All the numbers have 6 as their largest digit, so the answer is 2536 + 3366 = 5902.
```

**Example 3**

```
Input: nums = [51,71,17,24,42]

Output: 88

Explanation:

Each number's largest digit in order is [5,7,7,4,4].

So we have only two possible pairs, 71 + 17 = 88 and 24 + 42 = 66.
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
 * @Date 2025/07/27
 */
public class MaxNumberOfKSumPairs {

    public static int maxOperations(int[] nums, int k) {
        // This HashMap will store the count of each number that has not yet been paired.
        Map<Integer, Integer> countMap = new HashMap<>();
        int result = 0; // This variable counts the number of valid pairs found.

        // Iterate through each number in the array.
        for (int num: nums) {
            int target = k - num; // The number we need to pair with 'num' to sum to k.

            // Check if there is an available 'target' number that has not been paired yet.
            // countMap.getOrDefault(target, 0) > 0 means there is at least one 'target' left to pair.
            if (countMap.getOrDefault(target, 0) > 0){
                result++; // We found a valid pair (num + target == k), so increment the result.

                // Since we've used one 'target' for pairing, decrease its count by 1.
                // This ensures each number is used at most once.
                countMap.put(target, countMap.get(target) - 1);
            }else{
                // If there is no available 'target' to pair with 'num',
                // record 'num' in the map for possible pairing with future numbers.
                // Increase the count of 'num' by 1.
                countMap.put(num, countMap.getOrDefault(num, 0) + 1);
            }
        }

        // After processing all numbers, 'result' contains the maximum number of valid pairs.
        return result;
    }

    public static void main(String[] args) {
        int[] nums1 = {1, 2, 3, 4};
        int[] nums2 = {3, 1, 3, 4, 3};
        int[] nums3 = {2, 2, 2, 2};
        System.out.println(maxOperations(nums1, 5)); // Output: 2
        System.out.println(maxOperations(nums2, 6)); // Output: 1
        System.out.println(maxOperations(nums3, 4)); // Output: 2
    }
}

```





