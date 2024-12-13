---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2593. Find Score of an Array After Marking All Elements"
date: "2024-12-13"
tags: Medium
categories:
  - "LeetCode Array"
---

- You are given an array `nums` consisting of positive integers.
- Starting with `score = 0`, apply the following algorithm:
  - Find the **minimum** value `x` in `nums`. If there are multiple occurrences of the minimum value, select the one that appears **first**.
  - Replace the selected minimum value `x` with `x * multiplier`.
- Return *the score you get after applying the above algorithm*.

**Example 1**

```
Input: nums = [2,1,3,4,5,2]
Output: 7
Explanation: We mark the elements as follows:
- 1 is the smallest unmarked element, so we mark it and its two adjacent elements: [2,1,3,4,5,2].
- 2 is the smallest unmarked element, so we mark it and its left adjacent element: [2,1,3,4,5,2].
- 4 is the only remaining unmarked element, so we mark it: [2,1,3,4,5,2].
Our score is 1 + 2 + 4 = 7.
```

**Example 2**

```
Input: nums = [2,3,5,1,3,2]
Output: 5
Explanation: We mark the elements as follows:
- 1 is the smallest unmarked element, so we mark it and its two adjacent elements: [2,3,5,1,3,2].
- 2 is the smallest unmarked element, since there are two of them, we choose the left-most one, so we mark the one at index 0 and its right adjacent element: [2,3,5,1,3,2].
- 2 is the only remaining unmarked element, so we mark it: [2,3,5,1,3,2].
Our score is 1 + 2 + 2 = 5.
```

## Method 1

```tex
【O(nlog(n)) time | O(n) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2024/12/13
 */
public class FindScoreOfAnArrayAfterMarkingAllElements {
    public static long findScore(int[] nums) {
        // Get the length of the input array
        int n = nums.length;

        // Create an integer array to store the indices of the original array
        Integer[] ids = new Integer[n];

        // Initialize the index array with their corresponding indices
        for (int i = 0; i < n; i++) {
            ids[i] = i;
        }

        // Sort the index array based on the values in the original array
        // This allows us to process elements in ascending order of their values
        Arrays.sort(ids, (i, j) -> nums[i] - nums[j]);

        // Initialize the total score
        long ans = 0;

        // Create a boolean array to mark visited elements.
        // The extra two elements are used to handle boundary cases.
        boolean[] visited = new boolean[n + 2];

        // Iterate over the sorted indices
        for (int i : ids) {
            // If the current element and its right neighbor haven't been visited
            if (!visited[i + 1]) {
                // Mark the current element and its two neighbors as visited
                visited[i] = visited[i + 2] = true;

                // Add the value of the current element to the total score
                ans += nums[i];
            }
        }

        // Return the total score
        return ans;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {2,3,5,1,3,2};
        long result1 = findScore(nums1);
        System.out.println("Test Case 1 Result: " + result1);

        // Test case 2
        int[] nums2 = {2,1,3,4,5,2};
        long result2 = findScore(nums2);
        System.out.println("Test Case 2 Result: " + result2);
    }
}

```





