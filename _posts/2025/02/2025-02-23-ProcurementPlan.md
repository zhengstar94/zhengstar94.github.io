---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "LCP 28. Procurement Plan"
date: "2025-02-23"
tags: Easy
categories:
  - "LeetCode TwoPointers"
---

- Xiao Li stores the quotations of N parts in the array nums. Xiao Li's budget is target. Assuming Xiao Li only purchases two parts, and the cost of purchasing parts must not exceed the budget, how many procurement plans does he have?
- Note: The answer needs to be modulo `1e9 + 7 (1000000007)`. For example: if the initial result is `1000000008`, please return `1`.


**Example 1**

```
Input: nums = [2,5,3,5], target = 6
Output: 1
Explanation: Within the budget, only nums[0] and nums[2] can be purchased.
```

**Example 2**

```
Input: nums = [2,2,1,9], target = 10
Output: 4
Explanation: The procurement plans within budget are as follows:
nums[0] + nums[1] = 4
nums[0] + nums[2] = 3
nums[1] + nums[2] = 3
nums[2] + nums[3] = 10
```

## Method 1

```tex
【O(nlogn) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/02/23
 */
public class ProcurementPlan {
    // Constant for modulo operation to handle large numbers
    private static final int MOD = 1000000007;

    public static int purchasePlans(int[] nums, int target) {
        // Sort array to use two-pointer technique effectively
        Arrays.sort(nums);

        // Initialize two pointers and result counter
        int left = 0;                      // Left pointer starts from beginning
        int right = nums.length - 1;       // Right pointer starts from end
        long count = 0;                    // Use long to prevent overflow during calculation

        // Continue until pointers meet
        while (left < right) {
            // If current combination is within budget
            if (nums[left] + nums[right] <= target) {
                // Add number of valid combinations with current left pointer
                // (right - left) represents all possible combinations with current left value
                count = (count + right - left) % MOD;
                left++;    // Move left pointer right to try next combination
            } else {
                right--;   // Sum too large, decrease right pointer to reduce sum
            }
        }

        return (int)count;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Basic case with small numbers
        int[] nums1 = {2, 5, 3, 5};
        int target1 = 6;
        System.out.println("Test Case 1:");
        System.out.println("Input: nums = [2,5,3,5], target = 6");
        System.out.println("Output: " + purchasePlans(nums1, target1));
        System.out.println("Expected: 1");
        System.out.println();

        // Test Case 2: Multiple valid combinations
        int[] nums2 = {2, 2, 1, 9};
        int target2 = 10;
        System.out.println("Test Case 2:");
        System.out.println("Input: nums = [2,2,1,9], target = 10");
        System.out.println("Output: " + purchasePlans(nums2, target2));
        System.out.println("Expected: 4");
        System.out.println();

        // Test Case 3: Large dataset to test performance and overflow handling
        int[] nums3 = new int[100000];
        for (int i = 0; i < nums3.length; i++) {
            nums3[i] = i + 1;
        }
        int target3 = 100000;
        System.out.println("Test Case 3 (Large Dataset Test):");
        System.out.println("Output: " + purchasePlans(nums3, target3));
    }
}

```





