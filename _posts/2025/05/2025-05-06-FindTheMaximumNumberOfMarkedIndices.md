---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2576. Find the Maximum Number of Marked Indices"
date: "2025-05-06"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.FindMaximum"
---


- You are given a **0-indexed** integer array `nums`.
- Initially, all of the indices are unmarked. You are allowed to make this operation any number of times:
  - Pick two **different unmarked** indices `i` and `j` such that `2 * nums[i] <= nums[j]`, then mark `i` and `j`.
- Return *the maximum possible number of marked indices in `nums` using the above operation any number of times*.

**Example 1**

```
Input: nums = [3,5,2,4]
Output: 2
Explanation: In the first operation: pick i = 2 and j = 1, the operation is allowed because 2 * nums[2] <= nums[1]. Then mark index 2 and 1.
It can be shown that there's no other valid operation so the answer is 2.
```

**Example 2**

```
Input: nums = [9,2,5,4]
Output: 4
Explanation: In the first operation: pick i = 3 and j = 0, the operation is allowed because 2 * nums[3] <= nums[0]. Then mark index 3 and 0.
In the second operation: pick i = 1 and j = 2, the operation is allowed because 2 * nums[1] <= nums[2]. Then mark index 1 and 2.
Since there is no other operation, the answer is 4.
```

**Example 3**

```
Input: nums = [7,6,8]
Output: 0
Explanation: There is no valid operation to do, so the answer is 0.
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.BinarySearch.FindMaximum;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/05/06
 */
public class FindTheMaximumNumberOfMarkedIndices {

    public int maxNumOfMarkedIndices(int[] nums) {
        // Sort the array to enable our optimal pairing strategy
        Arrays.sort(nums);

        // Initialize binary search boundaries
        // left: current best answer (starts at 0)
        int left = 0;
        // right: theoretical upper bound (n/2 + 1) - we use an open interval approach
        // The maximum possible number of pairs is n/2 (where n is the array length)
        // We add 1 to make it an exclusive upper bound for the binary search
        int right = nums.length / 2 + 1;

        // Binary search for the maximum valid k value
        // We use (left + 1 < right) as our loop condition to avoid infinite loops
        // This approach will terminate when left and right are adjacent
        while (left + 1 < right) {
            // Calculate the middle point safely to avoid integer overflow
            int mid = left + (right - left) / 2;

            // Check if we can form 'mid' pairs that satisfy our condition
            if (check(nums, mid)) {
                // If yes, update our current best answer and try for more pairs
                left = mid;
            } else {
                // If no, reduce the upper bound of our search space
                right = mid;
            }
        }

        // Return the total number of marked indices
        // Each valid pair allows us to mark 2 indices, so we multiply by 2
        return left * 2;
    }

    private boolean check(int[] nums, int k) {
        // Check all k potential pairs
        for (int i = 0; i < k; i++) {
            // For each pair, check if the condition 2*nums[i] <= nums[j] is satisfied
            // nums[i]: the i-th smallest element
            // nums[nums.length - k + i]: the i-th element from the k largest elements
            //
            // Why nums.length - k + i?
            // - nums.length - k: This points to the starting position of the k largest elements
            // - Adding i: As i increases from 0 to k-1, we move through these k largest elements
            //
            // For example, with array [2,3,5,7,10,12] and k=3:
            // i=0: compare nums[0]=2 with nums[6-3+0]=nums[3]=7
            // i=1: compare nums[1]=3 with nums[6-3+1]=nums[4]=10
            // i=2: compare nums[2]=5 with nums[6-3+2]=nums[5]=12
            if (nums[i] * 2 > nums[nums.length - k + i]) {
                // If any pair doesn't satisfy the condition, we can't form k valid pairs
                return false;
            }
        }
        // All k pairs satisfy the condition
        return true;
    }

    public static void main(String[] args) {
        FindTheMaximumNumberOfMarkedIndices solution = new FindTheMaximumNumberOfMarkedIndices();

        // Test Case 1
        int[] nums1 = {3, 5, 2, 4};
        int expected1 = 2;
        int result1 = solution.maxNumOfMarkedIndices(nums1);
        System.out.println("Test Case 1: " + (result1 == expected1 ? "PASSED" : "FAILED") +
                " (Expected: " + expected1 + ", Got: " + result1 + ")");
        // After sorting: [2, 3, 4, 5]
        // We can form 1 pair: (2,5) where 2*2 <= 5, marking 2 indices

        // Test Case 2
        int[] nums2 = {9, 2, 5, 4};
        int expected2 = 4;
        int result2 = solution.maxNumOfMarkedIndices(nums2);
        System.out.println("Test Case 2: " + (result2 == expected2 ? "PASSED" : "FAILED") +
                " (Expected: " + expected2 + ", Got: " + result2 + ")");
        // After sorting: [2, 4, 5, 9]
        // We can form 2 pairs: (2,5) and (4,9), marking 4 indices

        // Test Case 3
        int[] nums3 = {7, 6, 8};
        int expected3 = 0;
        int result3 = solution.maxNumOfMarkedIndices(nums3);
        System.out.println("Test Case 3: " + (result3 == expected3 ? "PASSED" : "FAILED") +
                " (Expected: " + expected3 + ", Got: " + result3 + ")");
        // After sorting: [6, 7, 8]
        // We can't form any valid pairs because 2*6 > 8, so 0 indices are marked

        // Test Case 4: Longer array example to illustrate the pairing strategy
        int[] nums4 = {2, 3, 5, 7, 10, 12};
        int expected4 = 6;
        int result4 = solution.maxNumOfMarkedIndices(nums4);
        System.out.println("Test Case 4: " + (result4 == expected4 ? "PASSED" : "FAILED") +
                " (Expected: " + expected4 + ", Got: " + result4 + ")");
        // In this case (already sorted): [2, 3, 5, 7, 10, 12]
        // We can form 3 pairs: (2,7), (3,10), and (5,12), marking 6 indices
    }
}

```





