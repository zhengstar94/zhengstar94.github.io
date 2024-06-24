---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "53.Maximum Subarray"
date: "2024-06-01"
categories:
  - "LeetCode Greedy"
---

# 53. Maximum Subarray

- Given an integer array `nums`, find the subarray with the largest sum, and return *its sum*.

**Example 1**

```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: The subarray [4,-1,2,1] has the largest sum 6.
```

**Example 2**

```
Input: nums = [1]
Output: 1
Explanation: The subarray [1] has the largest sum 1.
```

**Example 3**

```
Input: nums = [5,4,-1,7,8]
Output: 23
Explanation: The subarray [5,4,-1,7,8] has the largest sum 23.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Greedy;

/**
 * @author zhengstars
 * @date 2024/06/13
 */
public class MaximumSubarray {
    public static int maxSubArray(int[] nums) {
        // Initialize current subarray sum and maximum subarray sum with the first element of the array.
        int currentSum = nums[0];
        int maxSum = nums[0];

        // Traverse the array starting from the second element.
        for (int i = 1; i < nums.length; i++) {
            // Update currentSum to be either the current element or the largest sum of the current subarray sum plus the current element.
            currentSum = Math.max(nums[i], currentSum + nums[i]);
            // Update maxSum to be the maximum subarray sum.
            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
        System.out.println("Max subarray sum for nums1: " + maxSubArray(nums1)); // Expected output: 6

        // Test case 2
        int[] nums2 = {1};
        System.out.println("Max subarray sum for nums2: " + maxSubArray(nums2)); // Expected output: 1

        // Test case 3
        int[] nums3 = {5, 4, -1, 7, 8};
        System.out.println("Max subarray sum for nums3: " + maxSubArray(nums3)); // Expected output: 23
    }
}
```

