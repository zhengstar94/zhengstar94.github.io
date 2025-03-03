---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2161. Partition Array According to Given Pivot"
date: "2025-03-03"
tags: Medium
categories:
  - "LeetCode Array" 
---


- You are given a **0-indexed** integer array `nums` and an integer `pivot`. Rearrange `nums` such that the following conditions are satisfied:
  - Every element less than `pivot` appears **before** every element greater than `pivot`.
  - Every element equal to `pivot` appears **in between** the elements less than and greater than `pivot`.
  - The **relative order** of the elements less than `pivot` and the elements greater than `pivot` is maintained.
    - More formally, consider every `pi`, `pj` where `pi` is the new position of the `ith` element and `pj` is the new position of the `jth` element. If `i < j` and **both** elements are smaller (*or larger*) than `pivot`, then `pi < pj`.
- Return `nums` *after the rearrangement.*

**Example 1**

```
Input: nums = [9,12,5,10,14,3,10], pivot = 10
Output: [9,5,3,10,10,12,14]
Explanation: 
The elements 9, 5, and 3 are less than the pivot so they are on the left side of the array.
The elements 12 and 14 are greater than the pivot so they are on the right side of the array.
The relative ordering of the elements less than and greater than pivot is also maintained. [9, 5, 3] and [12, 14] are the respective orderings.
```

**Example 2**

```
Input: nums = [-3,4,3,2], pivot = 2
Output: [-3,2,4,3]
Explanation: 
The element -3 is less than the pivot so it is on the left side of the array.
The elements 4 and 3 are greater than the pivot so they are on the right side of the array.
The relative ordering of the elements less than and greater than pivot is also maintained. [-3] and [4, 3] are the respective orderings.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/03/03
 */
public class PartitionArrayAccordingToGivenPivot {
    
    public static int[] pivotArray(int[] nums, int pivot) {
        // Get the length of input array
        int n = nums.length;
        // Create result array to store the partitioned elements
        int[] result = new int[n];

        // Pointer for placing elements in result array
        int left = 0;
        // Counter for elements equal to pivot
        int equal = 0;

        // First pass: Handle elements less than pivot
        for (int i = 0; i < n; i++) {
            if (nums[i] < pivot) {
                // Place smaller elements at the beginning of result array
                result[left++] = nums[i];
            } else if (nums[i] == pivot) {
                // Count elements equal to pivot
                equal++;
            }
        }

        // Place all elements equal to pivot
        for (int i = 0; i < equal; i++) {
            result[left++] = pivot;
        }

        // Second pass: Handle elements greater than pivot
        for (int i = 0; i < n; i++) {
            if (nums[i] > pivot) {
                // Place larger elements at the end of result array
                result[left++] = nums[i];
            }
        }

        return result;
    }
    
    public static void main(String[] args) {
        // Test Case 1
        int[] nums1 = {9, 12, 5, 10, 14, 3, 10};
        int pivot1 = 10;
        System.out.println("Test Case 1 Result: " + Arrays.toString(pivotArray(nums1, pivot1)));
        // Expected output: [9,5,3,10,10,12,14]

        // Test Case 2
        int[] nums2 = {-3, 4, 3, 2};
        int pivot2 = 2;
        System.out.println("Test Case 2 Result: " + Arrays.toString(pivotArray(nums2, pivot2)));
        // Expected output: [-3,2,4,3]
    }
}

```





