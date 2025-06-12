---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3423. Maximum Difference Between Adjacent Elements in a Circular Array"
date: "2025-06-12"
tags: Easy
categories:
  - "LeetCode Array"
---



- Given a **circular** array `nums`, find the **maximum** absolute difference between adjacent elements.
- **Note**: In a circular array, the first and last elements are adjacent.

**Example 1**

```
Input: nums = [1,2,4]

Output: 3

Explanation:

Because nums is circular, nums[0] and nums[2] are adjacent. They have the maximum absolute difference of |4 - 1| = 3.
```

**Example 2**

```
Input: nums = [-5,-10,-5]

Output: 5

Explanation:

The adjacent elements nums[0] and nums[1] have the maximum absolute difference of |-5 - (-10)| = 5.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @Author zhengxingxing
 * @Date 2025/06/12
 */
public class MaximumDifferenceBetweenAdjacentElementsInACircularArray {
    
    public static int maxAdjacentDistance(int[] nums) {
        // Get the length of the array
        int n = nums.length;

        // Initialize the answer with the absolute difference between the first and last elements
        int ans = Math.abs(nums[0] - nums[n - 1]);

        // Loop through the array starting from the second element
        for (int i = 1; i < n; i++) {
            // Update the answer with the maximum of the current answer and the absolute difference
            // between the current element and the previous element
            ans = Math.max(ans, Math.abs(nums[i] - nums[i - 1]));
        }

        // Return the maximum absolute difference found
        return ans;
    }


    public static void main(String[] args) {
        // Test case 1: An array with three elements
        int[] nums1 = new int[]{1, 2, 4};
        // Print the maximum adjacent distance for the first test case
        System.out.println(maxAdjacentDistance(nums1));

        // Test case 2: An array with negative numbers
        int[] nums2 = new int[]{-5, -10, -5};
        // Print the maximum adjacent distance for the second test case
        System.out.println(maxAdjacentDistance(nums2));
    }
}

```





