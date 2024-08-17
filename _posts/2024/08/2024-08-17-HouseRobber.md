---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "198.House Robber"
date: "2024-08-17"
categories:
  - "LeetCode Dynamic Programming"
---


# 198. House Robber

- You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.
- Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

**Example 1**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 2**

```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * @author zhengstars
 * @date 2024/08/17
 */
public class HouseRobber {
    public static int rob(int[] nums) {
        // Handle edge cases
        if (nums.length == 0) {
            return 0;
        }

        if (nums.length == 1) {
            return nums[0];
        }

        // Initialize the dp array
        int[] dp = new int[nums.length];
        // Only one house to rob
        dp[0] = nums[0];
        // Two houses to choose the maximum between
        dp[1] = Math.max(nums[0], nums[1]);

        // Fill the dp array based on the above rules
        for (int i = 2; i < nums.length; i++) {
            // dp[i] represents the maximum amount of money that can be robbed from the first i + 1 houses.
            // To determine dp[i], we have two choices:
            // 1. Rob the current house (nums[i]). In this case, we cannot rob the previous house (i-1), so the maximum amount is nums[i] + dp[i-2].
            // 2. Do not rob the current house. The maximum amount in this case would be the same as if we had only considered the first i houses, which is dp[i-1].

            // Choose the maximum amount from the two options above
            dp[i] = Math.max(nums[i] + dp[i-2], dp[i-1]);
        }

        // Return the result from the last house
        return dp[nums.length - 1];
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {1, 2, 3, 1};
        int maxRob1 = rob(nums1);
        System.out.println(maxRob1); // Expected output: 4

        // Test case 2
        int[] nums2 = {2, 7, 9, 3, 1};
        int maxRob2 = rob(nums2);
        System.out.println(maxRob2); // Expected output: 12

        // Test case 3
        int[] nums3 = {2, 1, 1, 2};
        int maxRob3 = rob(nums3);
        System.out.println(maxRob3); // Expected output: 4
    }
}

```

