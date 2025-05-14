---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2560. House Robber IV"
date: "2025-05-14"
tags: Medium BinarySearch
categories:
  - "LeetCode BinarySearch.MinimizeMaximum"
---

- There are several consecutive houses along a street, each of which has some money inside. There is also a robber, who wants to steal money from the homes, but he **refuses to steal from adjacent homes**.
- The **capability** of the robber is the maximum amount of money he steals from one house of all the houses he robbed.
- You are given an integer array `nums` representing how much money is stashed in each house. More formally, the `ith` house from the left has `nums[i]` dollars.
- You are also given an integer `k`, representing the **minimum** number of houses the robber will steal from. It is always possible to steal at least `k` houses.
- Return *the **minimum** capability of the robber out of all the possible ways to steal at least* `k` *houses*.

**Example 1**

```
Input: nums = [2,3,5,9], k = 2
Output: 5
Explanation: 
There are three ways to rob at least 2 houses:
- Rob the houses at indices 0 and 2. Capability is max(nums[0], nums[2]) = 5.
- Rob the houses at indices 0 and 3. Capability is max(nums[0], nums[3]) = 9.
- Rob the houses at indices 1 and 3. Capability is max(nums[1], nums[3]) = 9.
Therefore, we return min(5, 9, 9) = 5.
```

**Example 2**

```
Input: nums = [2,7,9,3,1], k = 2
Output: 2
Explanation: There are 7 ways to rob the houses. The way which leads to minimum capability is to rob the house at index 0 and 4. Return max(nums[0], nums[4]) = 2.
```

## Method 1

```tex
【O(n * log(max(nums) - min(nums))) time | O(1) space】
```

```java
package Leetcode.BinarySearch.MinimizeMaximum;

/**
 * @author zhengxingxing
 * @date 2025/05/14
 */
public class HouseRobberIV {

    public static int minCapability(int[] nums, int k) {
        // Initialize the binary search boundaries
        // Left boundary is the minimum value in the array (minimum possible capability)
        // Right boundary is the maximum value in the array (maximum possible capability)
        int left = Integer.MAX_VALUE;
        int right = 0;

        // Find the minimum and maximum values in the array
        for (int num : nums) {
            left = Math.min(left, num);
            right = Math.max(right, num);
        }

        // Binary search process to find the minimum capability
        while (left < right) {
            // Calculate the middle point to check
            int mid = left + (right - left) / 2;

            // Check if with capability 'mid', the robber can steal at least k houses
            if (canRob(nums, mid, k)) {
                // If it's possible to rob k houses with current capability,
                // try to lower the capability to find the minimum
                right = mid;
            } else {
                // If it's not possible to rob k houses with current capability,
                // we need to increase the capability
                left = mid + 1;
            }
        }

        // After binary search, 'left' will be the minimum capability required
        return left;
    }

    private static boolean canRob(int[] nums, int capability, int k) {
        int count = 0;  // Count of houses that have been robbed
        int i = 0;      // Current index in the array

        while (i < nums.length) {
            // If the money in the current house is less than or equal to the capability,
            // the robber can steal from this house
            if (nums[i] <= capability) {
                count++;  // Increment the count of robbed houses
                i += 2;   // Skip the next house (can't rob adjacent houses)
            } else {
                i++;      // Can't rob this house, move to the next one
            }
        }

        // Check if the robber has stolen from at least k houses
        return count >= k;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {2, 3, 5, 9};
        int k1 = 2;
        System.out.println("Test case 1 result: " + minCapability(nums1, k1));  // Expected output: 5

        // Test case 2
        int[] nums2 = {2, 7, 9, 3, 1};
        int k2 = 2;
        System.out.println("Test case 2 result: " + minCapability(nums2, k2));  // Expected output: 2

        // Additional test case
        int[] nums3 = {1, 2, 3, 4, 5, 6, 7, 8, 9};
        int k3 = 4;
        System.out.println("Test case 3 result: " + minCapability(nums3, k3));  // Expected output: 7
    }
}
```





