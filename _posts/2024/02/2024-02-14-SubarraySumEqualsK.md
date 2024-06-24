---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "560.Subarray Sum Equals K"
date: "2024-02-14"
categories:
  - "LeetCode HashTable"
---

# LeetCode 560. Subarray Sum Equals K 

- Given an array of integers `nums` and an integer `k`, return *the total number of subarrays whose sum equals to* `k`.

  A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1**

```
Input: nums = [1,1,1], k = 2
Output: 2
```

**Example 2**

```
Input: nums = [1,2,3], k = 3
Output: 2
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengstars
 * @date 2024/02/16
 */
public class SubarraySumEqualsK {
    public static int subarraySum(int[] nums, int k) {
        // Initialize the sum to start from 0
        int sum = 0;

        // Initialize the result to start from 0
        int result = 0;

        // Create a new HashMap to store the cumulative sum and its frequency
        Map<Integer, Integer> preSum = new HashMap<>();

        // The cumulative sum of 0 occurs once before the first number
        preSum.put(0, 1);

        // Iterate through the array
        for (int i = 0; i < nums.length; i++) {
            // Add the current number to the cumulative sum
            sum += nums[i];

            // Check if 'sum - k' exists in the HashMap
            // 'sum - k' means the sum before the subarray whose sum equals to k
            if (preSum.containsKey(sum - k)) {
                // If it exists, add its frequency to result
                result += preSum.get(sum - k);
            }

            // Update the frequency of the current cumulative sum in the HashMap
            preSum.put(sum, preSum.getOrDefault(sum, 0) + 1);
        }

        // Return the result, that sums up the frequencies of all valid sub-arrays
        return result;
    }

    public static void main(String[] args) {

        // Test case 1: a regular example
        // The expected output is 2
        // The array [1,1] and [1,1] have the sum equals to 2
        //System.out.println(subarraySum(new int[] {1,1,1}, 2));

        // Test case 2: another regular example
        // The expected output is 2
        // The array [1,2] and [3] have the sum equals to 3
        System.out.println(subarraySum(new int[] {1,2,3}, 3));
    }
}

```

