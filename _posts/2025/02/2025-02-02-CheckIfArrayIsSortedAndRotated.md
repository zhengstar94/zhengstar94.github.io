---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1752. Check if Array Is Sorted and Rotated"
date: "2025-02-02"
tags: Easy
categories:
  - "LeetCode Array"
---


- Given an array `nums`, return `true` *if the array was originally sorted in non-decreasing order, then rotated **some** number of positions (including zero)*. Otherwise, return `false`.
- There may be **duplicates** in the original array.
- **Note:** An array `A` rotated by `x` positions results in an array `B` of the same length such that `A[i] == B[(i+x) % A.length]`, where `%` is the modulo operation.

**Example 1**

```
Input: nums = [3,4,5,1,2]
Output: true
Explanation: [1,2,3,4,5] is the original sorted array.
You can rotate the array by x = 3 positions to begin on the the element of value 3: [3,4,5,1,2].
```

**Example 2**

```
Input: nums = [2,1,3,4]
Output: false
Explanation: There is no sorted array once rotated that can make nums.
```

**Example 3**

```
Input: nums = [1,2,3]
Output: true
Explanation: [1,2,3] is the original sorted array.
You can rotate the array by x = 0 positions (i.e. no rotation) to make nums.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/02/02
 */
public class CheckIfArrayIsSortedAndRotated {

    public static boolean check(int[] nums) {
        // Count the number of inversions (where current element > next element)
        int count = 0;
        int n = nums.length;

        // Check adjacent elements including first and last (circular check)
        for (int i = 0; i < n; i++) {
            if (nums[i] > nums[(i + 1) % n]) {
                count++;
            }
            // If more than one inversion is found, array cannot be sorted and rotated
            if (count > 1) {
                return false;
            }
        }

        return true;
    }

    public static void main(String[] args) {
        // Test Case 1: Array that is sorted and rotated
        int[] nums1 = {3, 4, 5, 1, 2};
        System.out.println("Test Case 1 Result: " + check(nums1)); // Should output true

        // Test Case 2: Array that cannot be sorted by rotation
        int[] nums2 = {2, 1, 3, 4};
        System.out.println("Test Case 2 Result: " + check(nums2)); // Should output false

        // Test Case 3: Already sorted array
        int[] nums3 = {1, 2, 3};
        System.out.println("Test Case 3 Result: " + check(nums3)); // Should output true

        // Test Case 4: Array with duplicate elements
        int[] nums4 = {1, 1, 1};
        System.out.println("Test Case 4 Result: " + check(nums4)); // Should output true
    }
}

```





