---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2357.Make Array Zero by Subtracting Equal Amounts"
date: "2024-02-02"
categories:
  - "LeetCode Array"
---

# LeetCode 2357. Make Array Zero by Subtracting Equal Amounts [Easy]

- You are given a non-negative integer array `nums`. In one operation, you must:
  - Choose a positive integer `x` such that `x` is less than or equal to the **smallest non-zero** element in `nums`.
  - Subtract `x` from every **positive** element in `nums`.
- Return *the **minimum** number of operations to make every element in* `nums` *equal to* `0`.

**Example 1**
```
Input: nums = [1,5,0,3,5]
Output: 3
Explanation:
In the first operation, choose x = 1. Now, nums = [0,4,0,2,4].
In the second operation, choose x = 2. Now, nums = [0,2,0,0,2].
In the third operation, choose x = 2. Now, nums = [0,0,0,0,0].
```

**Example 2**

```
Input: nums = [0]
Output: 0
Explanation: Each element in nums is already 0 so no operations are needed.
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Array;

import java.util.HashSet;
import java.util.Set;

/**
 * @author zhengstars
 * @date 2024/02/19
 */
public class MakeArrayZeroBySubtractingEqualAmounts {
    public static int minimumOperations(int[] nums) {
        // Create a set to store distinct positive numbers
        Set<Integer> set = new HashSet<>();

        // Traverse the array
        for (int a: nums) {
            // If the number is positive, add it to the set
            // Duplicates will be ignored automatically
            if (a > 0) {
                set.add(a);
            }
        }

        // The number of operations is the number of distinct positive numbers in the array
        return set.size();
    }

    public static void main(String[] args) {
        // Test the algorithm with test cases
        int[] nums1 = {1,5,0,3,5};
        System.out.println(minimumOperations(nums1));  // Expect output：3

        int[] nums2 = {0};
        System.out.println(minimumOperations(nums2));  // Expect output: 0
    }
}

```

