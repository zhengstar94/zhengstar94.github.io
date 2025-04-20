---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3350. Adjacent Increasing Subarrays Detection II"
date: "2025-04-20"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---

- Given an array `nums` of `n` integers, your task is to find the **maximum** value of `k` for which there exist **two** adjacent subarrays of length `k` each, such that both subarrays are **strictly** **increasing**. Specifically, check if there are **two** subarrays of length `k` starting at indices `a` and `b` (`a < b`), where:
  - Both subarrays `nums[a..a + k - 1]` and `nums[b..b + k - 1]` are **strictly increasing**.
  - The subarrays must be **adjacent**, meaning `b = a + k`.
- Return the **maximum** *possible* value of `k`.
- A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1**

```
Input: nums = [2,5,7,8,9,2,3,4,3,1]

Output: 3

Explanation:

The subarray starting at index 2 is [7, 8, 9], which is strictly increasing.
The subarray starting at index 5 is [2, 3, 4], which is also strictly increasing.
These two subarrays are adjacent, and 3 is the maximum possible value of k for which two such adjacent strictly increasing subarrays exist.
```

**Example 2**

```
Input: nums = [1,2,3,4,4,4,4,5,6,7]

Output: 2

Explanation:

The subarray starting at index 0 is [1, 2], which is strictly increasing.
The subarray starting at index 2 is [3, 4], which is also strictly increasing.
These two subarrays are adjacent, and 2 is the maximum possible value of k for which two such adjacent strictly increasing subarrays exist.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/04/20
 */
public class AdjacentIncreasingSubarraysDetectionII {

    public static int maxIncreasingSubarrays(List<Integer> nums) {
        // Initialize variables to track maximum length found so far
        int ans = 0;
        // Length of previous strictly increasing sequence
        int preCnt = 0;
        // Current position in array
        int i = 0;
        int n = nums.size();

        // Process array using grouped loop approach
        while (i < n){
            // Mark start of current increasing sequence
            int start = i;

            // Find end of current strictly increasing sequence
            while (i + 1 < n && nums.get(i) < nums.get(i + 1)){
                i++;
            }

            // Calculate length of current increasing sequence
            int cnt = i - start + 1;

            // Case 1: Split current sequence into two equal parts
            int splitLen = cnt / 2;
            // Case 2: Use adjacent sequences (current and previous)
            int adjacentLen = Math.min(preCnt, cnt);
            // Take maximum of the two cases
            int currentMax = Math.max(splitLen, adjacentLen);
            // Update global maximum if necessary
            ans = Math.max(ans, currentMax);

            // Current sequence becomes previous sequence for next iteration
            preCnt = cnt;
            i++;
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test Case 1: Sequence with multiple increasing subarrays
        List<Integer> test1 = Arrays.asList(1,2,3,4, 5,6, 7,8,9);
        System.out.println("Processing demonstration:");
        printProcess(test1);

        // Test Case 2: Sequence with one long increasing subarray
        List<Integer> test2 = Arrays.asList(1,2,3,4,5,6, 7,8);
        System.out.println("\nProcessing demonstration:");
        printProcess(test2);
    }


    public static void printProcess(List<Integer> nums) {
        int ans = 0;
        int preCnt = 0;
        int i = 0;

        while (i < nums.size()) {
            // Find current increasing sequence
            int start = i;
            while (i + 1 < nums.size() && nums.get(i) < nums.get(i + 1)) {
                i++;
            }
            int cnt = i - start + 1;

            // Print current sequence being processed
            System.out.println("\nCurrent sequence: " + nums.subList(start, i + 1));
            System.out.println("Current sequence length (cnt): " + cnt);
            System.out.println("Previous sequence length (preCnt): " + preCnt);

            // Print calculations for both cases
            int splitLen = cnt / 2;
            System.out.println("Case 1 (Split current sequence) = " + splitLen);

            int adjacentLen = Math.min(preCnt, cnt);
            System.out.println("Case 2 (Use adjacent sequences) = " + adjacentLen);

            int currentMax = Math.max(splitLen, adjacentLen);
            System.out.println("Current maximum = " + currentMax);

            ans = Math.max(ans, currentMax);
            System.out.println("Updated global maximum = " + ans);

            // Update for next iteration
            preCnt = cnt;
            i++;
        }
    }
}

```





