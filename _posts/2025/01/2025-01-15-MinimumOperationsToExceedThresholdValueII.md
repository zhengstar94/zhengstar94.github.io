---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3066. Minimum Operations to Exceed Threshold Value II"
date: "2025-01-15"
tags: Medium
categories:
  - "LeetCode Greedy"
---


- You are given a **0-indexed** integer array `nums`, and an integer `k`.
- In one operation, you will:
  - Take the two smallest integers `x` and `y` in `nums`.
  - Remove `x` and `y` from `nums`.
  - Add `min(x, y) * 2 + max(x, y)` anywhere in the array.
- **Note** that you can only apply the described operation if `nums` contains at least two elements.
- Return *the **minimum** number of operations needed so that all elements of the array are greater than or equal to* `k`.

**Example 1**

```
Input: nums = [2,11,10,1,3], k = 10
Output: 2
Explanation: In the first operation, we remove elements 1 and 2, then add 1 * 2 + 2 to nums. nums becomes equal to [4, 11, 10, 3].
In the second operation, we remove elements 3 and 4, then add 3 * 2 + 4 to nums. nums becomes equal to [10, 11, 10].
At this stage, all the elements of nums are greater than or equal to 10 so we can stop.
It can be shown that 2 is the minimum number of operations needed so that all elements of the array are greater than or equal to 10.
```

**Example 2**

```
Input: nums = [1,1,2,4,9], k = 20
Output: 4
Explanation: After one operation, nums becomes equal to [2, 4, 9, 3].
After two operations, nums becomes equal to [7, 4, 9].
After three operations, nums becomes equal to [15, 9].
After four operations, nums becomes equal to [33].
At this stage, all the elements of nums are greater than 20 so we can stop.
It can be shown that 4 is the minimum number of operations needed so that all elements of the array are greater than or equal to 20.
```

## Method 1

```tex
【O(nlog(n)) time | O(n) space】
```

```java
package Leetcode.Greedy;

import java.util.PriorityQueue;

/**
 * @author zhengxingxing
 * @date 2025/01/15
 */
public class MinimumOperationsToExceedThresholdValueII {
    public static int minOperations(int[] nums, int k) {
        // Create a min heap priority queue to always get the smallest elements
        PriorityQueue<Long> pq = new PriorityQueue<>();

        // Convert all elements to long and add them to the priority queue
        // This prevents integer overflow in calculations
        for (int num: nums) {
            pq.offer((long)num);
        }

        // Counter for the number of operations performed
        int operation = 0;

        // Continue while we have at least 2 elements and smallest element < k
        while (pq.size() >= 2 && pq.peek() < k) {
            // Remove the two smallest elements
            long x = pq.poll();
            long y = pq.poll();

            // Calculate new value according to the problem's formula
            long newVal = Math.min(x, y) * 2 + Math.max(x, y);

            // Add the new value back to the priority queue
            pq.offer(newVal);
            operation++;
        }

        // Return the number of operations if successful (smallest element >= k)
        // Otherwise return -1 indicating it's impossible to reach the threshold
        return pq.peek() >= k ? operation : -1;
    }

    public static void main(String[] args) {
        // Test Case 1
        // Expected result: 2 operations needed to make all elements >= 10
        int[] nums1 = {2,11,10,1,3};
        int k1 = 10;
        System.out.println("Test Case 1 Result: " + minOperations(nums1, k1));

        // Test Case 2
        // Expected result: 4 operations needed to make all elements >= 20
        int[] nums2 = {1,1,2,4,9};
        int k2 = 20;
        System.out.println("Test Case 2 Result: " + minOperations(nums2, k2));
    }
}

```





