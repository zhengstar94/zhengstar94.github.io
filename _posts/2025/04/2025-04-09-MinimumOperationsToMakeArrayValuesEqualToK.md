---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3375. Minimum Operations to Make Array Values Equal to K"
date: "2025-04-09"
tags: Easy
categories:
  - "LeetCode Math"
---

- You are given an integer array `nums` and an integer `k`.
- An integer `h` is called **valid** if all values in the array that are **strictly greater** than `h` are *identical*.
- For example, if `nums = [10, 8, 10, 8]`, a **valid** integer is `h = 9` because all `nums[i] > 9` are equal to 10, but 5 is not a **valid** integer.
- You are allowed to perform the following operation on `nums`:
  - Select an integer `h` that is *valid* for the **current** values in `nums`.
  - For each index `i` where `nums[i] > h`, set `nums[i]` to `h`.
- Return the **minimum** number of operations required to make every element in `nums` **equal** to `k`. If it is impossible to make all elements equal to `k`, return -1.

**Example 1**

```
Input: nums = [5,2,5,4,5], k = 2

Output: 2

Explanation:

The operations can be performed in order using valid integers 4 and then 2.
```

**Example 2**

```
Input: nums = [2,1,2], k = 2

Output: -1

Explanation:

It is impossible to make all the values equal to 2.
```

**Example 3**

```
Input: nums = [9,7,5,3], k = 1

Output: 4

Explanation:

The operations can be performed using valid integers in the order 7, 5, 3, and 1.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Math;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/04/09
 */
public class MinimumOperationsToMakeArrayValuesEqualToK {
    
    public static int minOperations(int[] nums, int k) {
        // Find the minimum value in the array
        int min = Arrays.stream(nums).min().getAsInt();

        // If k is greater than the minimum value, it's impossible to make all elements equal to k
        // because we can only decrease values, never increase them
        if(k > min){
            return -1;
        }

        // Count the number of distinct elements in the array
        // This represents the base number of operations needed
        int distinctCount = (int)Arrays.stream(nums).distinct().count();

        // If k equals the minimum value, we need one less operation
        // because the minimum value is already at the target
        return distinctCount - (k == min ? 1 : 0);
    }

    public static void main(String[] args) {
        // Test Case 1: Multiple operations needed
        // Expected: 2 operations to make all elements equal to 2
        int[] nums1 = {5, 2, 5, 4, 5};
        int k1 = 2;
        System.out.println("Test Case 1 Result: " + minOperations(nums1, k1)); // Expected output: 2

        // Test Case 2: Impossible case (contains element smaller than k)
        // Expected: -1 as we can't increase 1 to become 2
        int[] nums2 = {2, 1, 2};
        int k2 = 2;
        System.out.println("Test Case 2 Result: " + minOperations(nums2, k2)); // Expected output: -1

        // Test Case 3: Multiple distinct values
        // Expected: 4 operations to make all elements equal to 1
        int[] nums3 = {9, 7, 5, 3};
        int k3 = 1;
        System.out.println("Test Case 3 Result: " + minOperations(nums3, k3)); // Expected output: 4

        // Test Case 4: All elements already equal
        // Expected: 0 operations as all elements are already equal to k
        int[] nums4 = {5, 5, 5, 5};
        int k4 = 5;
        System.out.println("Test Case 4 Result: " + minOperations(nums4, k4)); // Expected output: 0

        // Test Case 5: Single element
        // Expected: 1 operation to reduce 10 to 5
        int[] nums5 = {10};
        int k5 = 5;
        System.out.println("Test Case 5 Result: " + minOperations(nums5, k5)); // Expected output: 1
    }
}

```





