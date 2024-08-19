---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "198.House Robber II"
date: "2024-08-19"
categories:
  - "LeetCode Dynamic Programming"
---

# 213. House Robber II

- You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.
- Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

**Example 1**

```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

**Example 2**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 3**

```
Input: nums = [1,2,3]
Output: 3
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.DynamicProgramming;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/08/19
 */
public class HouseRobberII {
    /**
     * Calculates the maximum amount of money that can be robbed from a circular street of houses
     * without alerting the police.
     * @param nums an integer array representing the amount of money in each house
     * @return the maximum amount of money that can be robbed
     */
    public static int rob(int[] nums) {
        // If there is only one house, rob it and return its value
        if (nums.length == 1) {
            return nums[0];
        }

        // Case 1: Rob houses from index 0 to n-2 (excluding the last house)
        // This array includes houses from the first house to the second-to-last house.
        int rob1 = robLinear(Arrays.copyOfRange(nums, 0, nums.length - 1));

        // Case 2: Rob houses from index 1 to n-1 (excluding the first house)
        // This array includes houses from the second house to the last house.
        int rob2 = robLinear(Arrays.copyOfRange(nums, 1, nums.length));

        // Return the maximum amount that can be robbed from either case
        return Math.max(rob1, rob2);
    }

    /**
     * Helper method to calculate the maximum amount of money that can be robbed
     * from a linear street of houses.
     * @param nums an integer array representing the amount of money in each house
     * @return the maximum amount of money that can be robbed
     */
    private static int robLinear(int[] nums) {
        int prev = 0, curr = 0;
        // Iterate through the array to compute the maximum amount of money that can be robbed
        for (int num : nums) {
            // Calculate the new maximum amount that can be robbed including the current house
            int temp = curr;
            curr = Math.max(prev + num, curr);
            prev = temp;
        }
        return curr;
    }

    public static void main(String[] args) {
        // Test case 1: Rob houses with amounts [2, 3, 2]
        // Expected output: 3 (rob house 2)
        int[] nums1 = {2, 3, 2};
        System.out.println("Maximum amount for [2, 3, 2]: " + rob(nums1));

        // Test case 2: Rob houses with amounts [1, 2, 3, 1]
        // Expected output: 4 (rob house 1 and house 3)
        int[] nums2 = {1, 2, 3, 1};
        System.out.println("Maximum amount for [1, 2, 3, 1]: " + rob(nums2));

        // Test case 3: Rob houses with amounts [1, 2, 3]
        // Expected output: 3 (rob house 3)
        int[] nums3 = {1, 2, 3};
        System.out.println("Maximum amount for [1, 2, 3]: " + rob(nums3));
    }
}

```

