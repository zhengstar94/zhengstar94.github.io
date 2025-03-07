---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "283.Move Zeroes"
date: "2024-10-26"
tags: Easy TwoPointers
categories:
  - "LeetCode SingleSeqInPlacePointers"
---

- Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.
- **Note** that you must do this in-place without making a copy of the array.

**Example 1**

```
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Example 2**

```
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengstars
 * @date 2024/10/26
 */
public class MoveZeroes {
    public static void moveZeroes(int[] nums) {
        int nonZeroIndex = 0;

        // First pass: Move all non-zero elements to the beginning of the array
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                nums[nonZeroIndex++] = nums[i];
            }
        }

        // Second pass: Fill the rest of the array with zeroes
        for (int i = nonZeroIndex; i < nums.length; i++) {
            nums[i] = 0;
        }
    }

    public static void main(String[] args) {
        int[] nums1 = {0, 1, 0, 3, 12};
        int[] nums2 = {0, 0, 1};

        System.out.println("Before moveZeroes:");
        printArray(nums1);
        moveZeroes(nums1);
        System.out.println("After moveZeroes:");
        printArray(nums1);

        System.out.println("\nBefore moveZeroes:");
        printArray(nums2);
        moveZeroes(nums2);
        System.out.println("After moveZeroes:");
        printArray(nums2);
    }

    private static void printArray(int[] nums) {
        for (int num : nums) {
            System.out.print(num + " ");
        }
        System.out.println();
    }
}

```





