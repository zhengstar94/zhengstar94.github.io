---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2270. Number of Ways to Split Array"
date: "2025-01-03"
tags: Medium
categories:
  - "LeetCode Array"
---


- You are given a **0-indexed** integer array `nums` of length `n`.
- `nums` contains a **valid split** at index `i` if the following are true:
  - The sum of the first `i + 1` elements is **greater than or equal to** the sum of the last `n - i - 1` elements.
  - There is **at least one** element to the right of `i`. That is, `0 <= i < n - 1`.
- Return *the number of **valid splits** in* `nums`.


**Example 1**

```
Input: nums = [10,4,-8,7]
Output: 2
Explanation: 
There are three ways of splitting nums into two non-empty parts:
- Split nums at index 0. Then, the first part is [10], and its sum is 10. The second part is [4,-8,7], and its sum is 3. Since 10 >= 3, i = 0 is a valid split.
- Split nums at index 1. Then, the first part is [10,4], and its sum is 14. The second part is [-8,7], and its sum is -1. Since 14 >= -1, i = 1 is a valid split.
- Split nums at index 2. Then, the first part is [10,4,-8], and its sum is 6. The second part is [7], and its sum is 7. Since 6 < 7, i = 2 is not a valid split.
Thus, the number of valid splits in nums is 2.
```

**Example 2**

```
Input: nums = [2,3,1,0]
Output: 2
Explanation: 
There are two valid splits in nums:
- Split nums at index 1. Then, the first part is [2,3], and its sum is 5. The second part is [1,0], and its sum is 1. Since 5 >= 1, i = 1 is a valid split. 
- Split nums at index 2. Then, the first part is [2,3,1], and its sum is 6. The second part is [0], and its sum is 0. Since 6 >= 0, i = 2 is a valid split.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/01/03
 */
public class NumberOfWaysToSplitArray {
    /**
     * Calculate the number of valid ways to split the array
     * @param nums input array
     * @return number of valid splits
     */
    public static int waysToSplitArray(int[] nums) {
        int n = nums.length;
        long totalSum = 0;  // Use long to prevent integer overflow
        long leftSum = 0;   // Sum of elements on the left side
        int validSpilt = 0; // Counter for valid splits

        // Calculate the total sum of the array
        for (int num: nums) {
            totalSum += num;
        }

        // Iterate through potential split points
        for (int i = 0; i < n - 1; i++) {  // n-1 because we need at least one element on the right
            leftSum += nums[i];
            long rightSum = totalSum - leftSum;

            // Check if current split is valid
            if(leftSum >= rightSum){
                validSpilt++;
            }
        }

        return validSpilt;
    }

    public static void main(String[] args) {
        // Test Case 1: Basic case with positive and negative numbers
        int[] nums1 = {10,4,-8,7};
        System.out.println("Test Case 1: " + waysToSplitArray(nums1)); // Expected output: 2

        // Test Case 2: Array with all positive numbers
        int[] nums2 = {2,3,1,0};
        System.out.println("Test Case 2: " + waysToSplitArray(nums2)); // Expected output: 2

        // Test Case 3: Edge case with large numbers and negative values
        int[] nums3 = {1000000000,1000000000,1000000000,-1000000000};
        System.out.println("Test Case 3: " + waysToSplitArray(nums3)); // Testing large numbers

        // Test Case 4: Boundary case with minimum length
        int[] nums4 = {1,1};
        System.out.println("Test Case 4: " + waysToSplitArray(nums4)); // Testing minimum length
    }
}
```





