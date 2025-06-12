---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2289. Steps to Make Array Non-decreasing"
date: "2025-06-12"
tags: Medium
categories:
  - "LeetCode Stack"
---


- You are given a **0-indexed** integer array `nums`. In one step, **remove** all elements `nums[i]` where `nums[i - 1] > nums[i]` for all `0 < i < nums.length`.
- Return *the number of steps performed until* `nums` *becomes a **non-decreasing** array*.

**Example 1**

```
Input: nums = [5,3,4,4,7,3,6,11,8,5,11]
Output: 3
Explanation: The following are the steps performed:
- Step 1: [5,3,4,4,7,3,6,11,8,5,11] becomes [5,4,4,7,6,11,11]
- Step 2: [5,4,4,7,6,11,11] becomes [5,4,7,11,11]
- Step 3: [5,4,7,11,11] becomes [5,7,11,11]
[5,7,11,11] is a non-decreasing array. Therefore, we return 3.
```

**Example 2**

```
Input: nums = [4,5,7,7,13]
Output: 0
Explanation: nums is already a non-decreasing array. Therefore, we return 0.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.ArrayDeque;

/**
 * @Author zhengxingxing
 * @Date 2025/06/12
 *
 * This class contains a method to calculate the total number of steps required
 * to make an array non-decreasing. It uses a stack-based algorithm to efficiently
 * solve the problem.
 */
public class StepsToMakeArrayNonDecreasing {

    /**
     * This method calculates the total number of steps required to make the input array non-decreasing.
     *
     * @param nums The input array of integers.
     * @return The total number of steps required.
     */
    public static int totalSteps(int[] nums) {
        // Variable to store the maximum number of steps required
        int ans = 0;

        // Stack to store pairs of numbers and their associated maximum steps
        ArrayDeque<int[]> st = new ArrayDeque<>();

        // Iterate through each number in the input array
        for (int num : nums) {
            // Variable to track the maximum steps for the current number
            int maxT = 0;

            // While the stack is not empty and the top of the stack is less than or equal to the current number
            while (!st.isEmpty() && st.peek()[0] <= num) {
                // Update maxT to the maximum steps of the top element in the stack
                maxT = Math.max(maxT, st.pop()[1]);
            }

            // If the stack is empty, maxT remains 0; otherwise, increment maxT by 1
            maxT = st.isEmpty() ? 0 : maxT + 1;

            // Update the overall maximum steps required
            ans = Math.max(ans, maxT);

            // Push the current number and its associated maxT onto the stack
            st.push(new int[]{num, maxT});
        }

        // Return the total maximum steps required
        return ans;
    }

    /**
     * Main method to test the totalSteps method with example test cases.
     */
    public static void main(String[] args) {
        // Test case 1: Example input
        int[] nums1 = {5, 3, 4, 4, 7, 3, 6, 11, 8, 5, 11};
        System.out.println("Test Case 1: " + totalSteps(nums1)); // Expected output: 3

        // Test case 2: Already non-decreasing array
        int[] nums2 = {4,5,7,7,13};
        System.out.println("Test Case 2: " + totalSteps(nums2)); // Expected output: 0

    }
}

```





