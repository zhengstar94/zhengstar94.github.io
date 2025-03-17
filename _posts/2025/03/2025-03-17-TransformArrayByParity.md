---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3467. Transform Array by Parity"
date: "2025-03-17"
tags: Easy TwoPointers
categories:
  - "LeetCode SingleSeqInPlacePointers" 
---

- You are given an integer array `nums`. Transform `nums` by performing the following operations in the **exact** order specified:
  1. Replace each even number with 0.
  2. Replace each odd numbers with 1.
  3. Sort the modified array in **non-decreasing** order.
- Return the resulting array after performing these operations.

**Example 1**

```
Input: nums = [4,3,2,1]

Output: [0,0,1,1]

Explanation:

Replace the even numbers (4 and 2) with 0 and the odd numbers (3 and 1) with 1. Now, nums = [0, 1, 0, 1].
After sorting nums in non-descending order, nums = [0, 0, 1, 1].
```

**Example 2**

```
Input: nums = [1,5,1,4,2]

Output: [0,0,1,1,1]

Explanation:

Replace the even numbers (4 and 2) with 0 and the odd numbers (1, 5 and 1) with 1. Now, nums = [1, 1, 1, 0, 0].
After sorting nums in non-descending order, nums = [0, 0, 1, 1, 1].
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer.SingleSeqInPlacePointers;

/**
 * @author zhengxingxing
 * @date 2025/03/17
 */
public class TransformArrayByParity {
    
    public static int[] transformArray(int[] nums) {
        // Count numbers that will become 0 (even numbers)
        int countZeros = 0;
        for (int num : nums) {
            if (num % 2 == 0) {
                countZeros++;
            }
        }

        // Create and populate result array
        int[] result = new int[nums.length];
        // Fill with 0s for even count
        for (int i = 0; i < countZeros; i++) {
            result[i] = 0;
        }
        // Fill with 1s for remaining positions
        for (int i = countZeros; i < nums.length; i++) {
            result[i] = 1;
        }

        return result;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Basic case with even and odd numbers
        int[] nums1 = {4,3,2,1};
        System.out.println("Test Case 1:");
        System.out.print("Input array: ");
        printArray(nums1);
        System.out.print("Output array: ");
        printArray(transformArray(nums1));

        // Test Case 2: Array with multiple odd numbers
        int[] nums2 = {1,5,1,4,2};
        System.out.println("\nTest Case 2:");
        System.out.print("Input array: ");
        printArray(nums2);
        System.out.print("Output array: ");
        printArray(transformArray(nums2));
    }
    
    private static void printArray(int[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) {
                System.out.print(",");
            }
        }
        System.out.println("]");
    }
}

```





