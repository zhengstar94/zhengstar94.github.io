---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2009. Minimum Number of Operations to Make Array Continuous"
date: "2025-01-25"
tags: Hard
categories:
  - "LeetCode DynamicSlidingWindowMax"
---


- You are given an integer array `nums`. In one operation, you can replace **any** element in `nums` with **any** integer.
- `nums` is considered **continuous** if both of the following conditions are fulfilled:
  - All elements in `nums` are **unique**.
  - The difference between the **maximum** element and the **minimum** element in `nums` equals `nums.length - 1`.
- For example, `nums = [4, 2, 5, 3]` is **continuous**, but `nums = [1, 2, 3, 5, 6]` is **not continuous**.
- Return *the **minimum** number of operations to make* `nums` ***continuous\***.

**Example 1**

```
Input: nums = [4,2,5,3]
Output: 0
Explanation: nums is already continuous.
```

**Example 2**

```
Input: nums = [1,2,3,5,6]
Output: 1
Explanation: One possible solution is to change the last element to 4.
The resulting array is [1,2,3,5,4], which is continuous.
```

**Example 3**

```
Input: nums = [1,10,100,1000]
Output: 3
Explanation: One possible solution is to:
- Change the second element to 2.
- Change the third element to 3.
- Change the fourth element to 4.
The resulting array is [1,2,3,4], which is continuous.
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/01/25
 */
public class MinimumNumberOfOperationsToMakeArrayContinuous {

    public static int minOperations(int[] nums) {
        int n = nums.length;

        // Step 1: Sort and remove duplicates
        // - Sort the array first to handle duplicates and make it easier to find continuous ranges
        // - This also helps in implementing the sliding window approach
        Arrays.sort(nums);
        int unique = 1;
        for (int i = 1; i < n; i++) {
            // Only keep unique elements by moving them to the front of the array
            if (nums[i] != nums[i-1]) {
                nums[unique++] = nums[i];
            }
        }

        // Step 2: Use sliding window to find minimum replacements needed
        // Initialize minOperations with n (worst case: need to replace all elements)
        int minOperations = n;
        int j = 0;

        // Iterate through each possible starting point
        for (int i = 0; i < unique; i++) {
            // Find the first number that's out of range for current window
            // For a continuous array of length n starting at nums[i],
            // all elements must be in range [nums[i], nums[i] + n - 1]
            while (j < unique && nums[j] < nums[i] + n) {
                j++;
            }

            // Calculate number of elements in current window
            // j - i represents the count of numbers that can be used in the continuous array
            int count = j - i;

            // Calculate minimum operations needed for this window
            // Total length (n) minus the count of usable numbers equals
            // the number of elements that need to be replaced
            minOperations = Math.min(minOperations, n - count);
        }

        return minOperations;
    }

    public static void main(String[] args) {
        // Test Case 1: Already continuous array
        // Expected output: 0 (no operations needed as [2,3,4,5] is already continuous)
        int[] nums1 = {4,2,5,3};
        System.out.println("Test Case 1 Result: " + minOperations(nums1));

        // Test Case 2: Array with one gap
        // Expected output: 1 (replace either 5 with 4 or 6 with 4)
        int[] nums2 = {1,2,3,5,6};
        System.out.println("Test Case 2 Result: " + minOperations(nums2));

        // Test Case 3: Array with large gaps
        // Expected output: 3 (need to replace 3 elements to make it continuous)
        int[] nums3 = {1,10,100,1000};
        System.out.println("Test Case 3 Result: " + minOperations(nums3));
    }
}

```





