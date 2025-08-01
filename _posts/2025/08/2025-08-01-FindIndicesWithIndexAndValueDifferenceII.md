---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2905. Find Indices With Index and Value Difference II"
date: "2025-08-01"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---

- You are given a **0-indexed** integer array `nums` having length `n`, an integer `indexDifference`, and an integer `valueDifference`.
- Your task is to find **two** indices `i` and `j`, both in the range `[0, n - 1]`, that satisfy the following conditions:
  - `abs(i - j) >= indexDifference`, and
  - `abs(nums[i] - nums[j]) >= valueDifference`
- Return *an integer array* `answer`, *where* `answer = [i, j]` *if there are two such indices*, *and* `answer = [-1, -1]` *otherwise*. If there are multiple choices for the two indices, return *any of them*.
- **Note:** `i` and `j` may be **equal**.

**Example 1**

```
Input: nums = [5,1,4,1], indexDifference = 2, valueDifference = 4
Output: [0,3]
Explanation: In this example, i = 0 and j = 3 can be selected.
abs(0 - 3) >= 2 and abs(nums[0] - nums[3]) >= 4.
Hence, a valid answer is [0,3].
[3,0] is also a valid answer.
```

**Example 2**

```
Input: nums = [2,1], indexDifference = 0, valueDifference = 0
Output: [0,0]
Explanation: In this example, i = 0 and j = 0 can be selected.
abs(0 - 0) >= 0 and abs(nums[0] - nums[0]) >= 0.
Hence, a valid answer is [0,0].
Other valid answers are [0,1], [1,0], and [1,1].
```

**Example 3**

```
Input: nums = [1,2,3], indexDifference = 2, valueDifference = 4
Output: [-1,-1]
Explanation: In this example, it can be shown that it is impossible to find two indices that satisfy both conditions.
Hence, [-1,-1] is returned.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @Author zhengxingxing
 * @Date 2025/08/01
 */
public class FindIndicesWithIndexAndValueDifferenceII {

    public static int[] findIndices(int[] nums, int indexDifference, int valueDifference) {
        // maxIdx: stores the index of the current maximum value in the sliding window
        int maxIdx = 0;
        // minIdx: stores the index of the current minimum value in the sliding window
        int minIdx = 0;

        // Start j from indexDifference to ensure abs(i - j) >= indexDifference
        for (int j = indexDifference; j < nums.length; j++) {
            // i is the left boundary of the window for current j
            int i = j - indexDifference;

            // If the new left boundary value is greater than the current max, update maxIdx
            if (nums[i] > nums[maxIdx]){
                maxIdx = i;
            }

            // If the new left boundary value is less than the current min, update minIdx
            if (nums[i] < nums[minIdx]){
                minIdx = i;
            }

            // Check if the difference between the current max in the window and nums[j] 
            // is at least valueDifference. If so, return the pair.
            if (nums[maxIdx] - nums[j] >= valueDifference) {
                return new int[]{maxIdx, j};
            }

            // Check if the difference between nums[j] and the current min in the window
            // is at least valueDifference. If so, return the pair.
            if (nums[j] - nums[minIdx] >= valueDifference) {
                return new int[]{minIdx, j};
            }
        }

        // If no such pair is found, return [-1, -1]
        return new int[]{-1, -1};
    }

    public static void main(String[] args) {
        int[] nums1 = {5, 1, 4, 1};
        int indexDifference1 = 2, valueDifference1 = 4;
        // Example 1: Should print [0, 3] or [3, 0] (any valid pair)
        System.out.println(java.util.Arrays.toString(findIndices(nums1, indexDifference1, valueDifference1)));

        int[] nums2 = {2, 1};
        int indexDifference2 = 0, valueDifference2 = 0;
        // Example 2: Should print [0, 0] or [1, 1] or any valid pair
        System.out.println(java.util.Arrays.toString(findIndices(nums2, indexDifference2, valueDifference2)));

        int[] nums3 = {1, 2, 3};
        int indexDifference3 = 2, valueDifference3 = 4;
        // Example 3: Should print [-1, -1] (no valid pair)
        System.out.println(java.util.Arrays.toString(findIndices(nums3, indexDifference3, valueDifference3)));
    }
}

```





