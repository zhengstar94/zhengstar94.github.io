---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2016. Maximum Difference Between Increasing Elements"
date: "2025-06-16"
tags: Easy
categories:
  - "LeetCode Array"
---


- Given a **0-indexed** integer array `nums` of size `n`, find the **maximum difference** between `nums[i]` and `nums[j]` (i.e., `nums[j] - nums[i]`), such that `0 <= i < j < n` and `nums[i] < nums[j]`.
- Return *the **maximum difference**.* If no such `i` and `j` exists, return `-1`.

**Example 1**

```
Input: nums = [7,1,5,4]
Output: 4
Explanation:
The maximum difference occurs with i = 1 and j = 2, nums[j] - nums[i] = 5 - 1 = 4.
Note that with i = 1 and j = 0, the difference nums[j] - nums[i] = 7 - 1 = 6, but i > j, so it is not valid.
```

**Example 2**

```
Input: nums = [9,4,3,2]
Output: -1
Explanation:
There is no i and j such that i < j and nums[i] < nums[j].
```

**Example 3**

```
Input: nums = [1,5,2,10]
Output: 9
Explanation:
The maximum difference occurs with i = 0 and j = 3, nums[j] - nums[i] = 10 - 1 = 9.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @Author zhengxingxing
 * @Date 2025/06/16
 */
public class MaximumDifferenceBetweenIncreasingElements {

    public static int maximumDifference(int[] nums) {
        // Initialize minVal to the first element.
        // This variable keeps track of the smallest value seen so far as we iterate.
        int minVal = nums[0];

        // Initialize maxDiff to -1.
        // This will store the maximum difference found that satisfies the condition.
        int maxDiff = -1;

        // Start iterating from the second element (index 1) to the end of the array.
        for (int i = 1; i < nums.length; i++) {
            // If the current element is greater than the minimum value seen so far,
            // it means nums[i] can be nums[j] and minVal can be nums[i] for some i < j.
            if (nums[i] > minVal){
                // Calculate the difference and update maxDiff if this difference is larger.
                maxDiff = Math.max(maxDiff, nums[i] - minVal);
            } else {
                // If the current element is less than or equal to minVal,
                // update minVal to the current element.
                // This ensures minVal always holds the smallest value up to the current index.
                minVal = nums[i];
            }
        }

        // After the loop, return the maximum difference found.
        // If no valid pair was found, maxDiff remains -1.
        return maxDiff;
    }

    public static void main(String[] args) {
        // Test case 1: Expected output is 4
        int[] nums1 = {7, 1, 5, 4};
        System.out.println("Test case 1 result: " + maximumDifference(nums1));

        // Test case 2: Expected output is -1 (no valid pair exists)
        int[] nums2 = {9, 4, 3, 2};
        System.out.println("Test case 2 result: " + maximumDifference(nums2));

        // Test case 3: Expected output is 9
        int[] nums3 = {1, 5, 2, 10};
        System.out.println("Test case 3 result: " + maximumDifference(nums3));
    }
}

```





