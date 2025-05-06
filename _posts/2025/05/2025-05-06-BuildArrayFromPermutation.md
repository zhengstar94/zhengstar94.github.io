---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1920. Build Array from Permutation"
date: "2025-05-06"
tags: Easy
categories:
  - "LeetCode Array"
---


- Given a **zero-based permutation** `nums` (**0-indexed**), build an array `ans` of the **same length** where `ans[i] = nums[nums[i]]` for each `0 <= i < nums.length` and return it.
- A **zero-based permutation** `nums` is an array of **distinct** integers from `0` to `nums.length - 1` (**inclusive**).

**Example 1**

```
Input: nums = [0,2,1,5,3,4]
Output: [0,1,2,4,5,3]
Explanation: The array ans is built as follows: 
ans = [nums[nums[0]], nums[nums[1]], nums[nums[2]], nums[nums[3]], nums[nums[4]], nums[nums[5]]]
    = [nums[0], nums[2], nums[1], nums[5], nums[3], nums[4]]
    = [0,1,2,4,5,3]
```

**Example 2**

```
Input: nums = [5,0,1,2,3,4]
Output: [4,5,0,1,2,3]
Explanation: The array ans is built as follows:
ans = [nums[nums[0]], nums[nums[1]], nums[nums[2]], nums[nums[3]], nums[nums[4]], nums[nums[5]]]
    = [nums[5], nums[0], nums[1], nums[2], nums[3], nums[4]]
    = [4,5,0,1,2,3]
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/05/06
 */
public class BuildArrayFromPermutation {
    
    public static int[] buildArray(int[] nums) {
        int n = nums.length;
        // Create result array with same length as input array
        int[] ans = new int[n];

        // Iterate through array and build result
        // For each position i, ans[i] = nums[nums[i]]
        for(int i = 0; i < n; i++) {
            ans[i] = nums[nums[i]];
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test Case 1: Expected output [0,1,2,4,5,3]
        int[] nums1 = {0,2,1,5,3,4};
        int[] result1 = buildArray(nums1);
        System.out.print("Test Case 1 Result: ");
        for(int num : result1) {
            System.out.print(num + " ");
        }
        System.out.println();

        // Test Case 2: Expected output [4,5,0,1,2,3]
        int[] nums2 = {5,0,1,2,3,4};
        int[] result2 = buildArray(nums2);
        System.out.print("Test Case 2 Result: ");
        for(int num : result2) {
            System.out.print(num + " ");
        }
    }
}

```





