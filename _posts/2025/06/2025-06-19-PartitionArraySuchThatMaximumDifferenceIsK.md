---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2294. Partition Array Such That Maximum Difference Is K"
date: "2025-06-19"
tags: Medium
categories:
  - "LeetCode Greedy"
---


- You are given an integer array `nums` and an integer `k`. You may partition `nums` into one or more **subsequences** such that each element in `nums` appears in **exactly** one of the subsequences.
- Return *the **minimum** number of subsequences needed such that the difference between the maximum and minimum values in each subsequence is **at most*** `k`*.*
- A **subsequence** is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

**Example 1**

```
Input: nums = [3,6,1,2,5], k = 2
Output: 2
Explanation:
We can partition nums into the two subsequences [3,1,2] and [6,5].
The difference between the maximum and minimum value in the first subsequence is 3 - 1 = 2.
The difference between the maximum and minimum value in the second subsequence is 6 - 5 = 1.
Since two subsequences were created, we return 2. It can be shown that 2 is the minimum number of subsequences needed.
```

**Example 2**

```
Input: nums = [1,2,3], k = 1
Output: 2
Explanation:
We can partition nums into the two subsequences [1,2] and [3].
The difference between the maximum and minimum value in the first subsequence is 2 - 1 = 1.
The difference between the maximum and minimum value in the second subsequence is 3 - 3 = 0.
Since two subsequences were created, we return 2. Note that another optimal solution is to partition nums into the two subsequences [1] and [2,3].
```

**Example 3**

```
Input: nums = [2,2,4,5], k = 0
Output: 3
Explanation:
We can partition nums into the three subsequences [2,2], [4], and [5].
The difference between the maximum and minimum value in the first subsequences is 2 - 2 = 0.
The difference between the maximum and minimum value in the second subsequences is 4 - 4 = 0.
The difference between the maximum and minimum value in the third subsequences is 5 - 5 = 0.
Since three subsequences were created, we return 3. It can be shown that 3 is the minimum number of subsequences needed.
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.Greedy;

import java.util.Arrays;

/**
 * @Author zhengxingxing
 * @Date 2025/06/19
 */
public class PartitionArraySuchThatMaximumDifferenceIsK {
    
    public static int partitionArray(int[] nums, int k) {
        // Sort the array so we can process elements in ascending order
        Arrays.sort(nums);

        int ans = 0; // Counter for the number of required subsequences

        // Initialize mn (the minimum value in the current group)
        // to a very small number to ensure the first element always starts a new group.
        // We use Integer.MIN_VALUE / 2 to avoid potential overflow in x - mn.
        int mn = Integer.MIN_VALUE / 2;

        // Traverse each number in the sorted array
        for (int x : nums) {
            // If the difference between the current number and the minimum value in
            // the current group exceeds k, we must start a new group
            if (x - mn > k) {
                ans++;   // Start a new group, increment the group counter
                mn = x;  // Set the current number as the new group's minimum
            }
            // Else, this number can be included in the current group,
            // and we do not update ans or mn.
        }

        return ans;
    }

    public static void main(String[] args) {
        int[] nums1 = {3, 6, 1, 2, 5};
        int k1 = 2;
        System.out.println("Output: " + partitionArray(nums1, k1)); // Output: 2

        int[] nums2 = {1, 2, 3};
        int k2 = 1;
        System.out.println("Output: " + partitionArray(nums2, k2)); // Output: 2

        int[] nums3 = {2, 2, 4, 5};
        int k3 = 0;
        System.out.println("Output: " + partitionArray(nums3, k3)); // Output: 3
    }
}

```





