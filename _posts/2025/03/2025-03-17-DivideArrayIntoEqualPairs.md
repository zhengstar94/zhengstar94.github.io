---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2206. Divide Array Into Equal Pairs"
date: "2025-03-17"
tags: Easy
categories:
  - "LeetCode Math"
---


- You are given an integer array `nums` consisting of `2 * n` integers.
- You need to divide `nums` into `n` pairs such that:
  - Each element belongs to **exactly one** pair.
  - The elements present in a pair are **equal**.
- Return `true` *if nums can be divided into* `n` *pairs, otherwise return* `false`.

**Example 1**

```
Input: nums = [3,2,3,2,2,2]
Output: true
Explanation: 
There are 6 elements in nums, so they should be divided into 6 / 2 = 3 pairs.
If nums is divided into the pairs (2, 2), (3, 3), and (2, 2), it will satisfy all the conditions.
```

**Example 2**

```
Input: nums = [1,2,3,4]
Output: false
Explanation: 
There is no way to divide nums into 4 / 2 = 2 pairs such that the pairs satisfy every condition.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/03/17
 */
public class DivideArrayIntoEqualPairs {
    
    public static boolean divideArray(int[] nums) {
        // Create counting array (constraint: nums[i] <= 500)
        int[] count = new int[501];

        // Count frequency of each number
        for (int num : nums) {
            count[num]++;
        }

        // Check if each number appears even number of times
        for (int c : count) {
            if (c % 2 != 0) {
                return false;  // Found a number with odd frequency
            }
        }

        return true;  // All numbers appear even number of times
    }
    
    public static void main(String[] args) {
        // Test Case 1: Should return true
        int[] nums1 = {3,2,3,2,2,2};
        System.out.println("Test Case 1: ");
        System.out.print("Input array: ");
        printArray(nums1);
        System.out.println("Expected result: true");
        System.out.println("Actual result: " + divideArray(nums1));

        // Test Case 2: Should return false
        int[] nums2 = {1,2,3,4};
        System.out.println("\nTest Case 2: ");
        System.out.print("Input array: ");
        printArray(nums2);
        System.out.println("Expected result: false");
        System.out.println("Actual result: " + divideArray(nums2));
    }
    
    private static void printArray(int[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) {
                System.out.print(", ");
            }
        }
        System.out.println("]");
    }
}

```





