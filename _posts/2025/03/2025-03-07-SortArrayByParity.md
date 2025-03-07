---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "905. Sort Array By Parity"
date: "2025-03-07"
tags: Easy TwoPointers
categories:
  - "LeetCode SingleSeqInPlacePointers" 
---


- Given an integer array `nums`, move all the even integers at the beginning of the array followed by all the odd integers.
- Return ***any array** that satisfies this condition*.

**Example 1**

```
Input: nums = [3,1,2,4]
Output: [2,4,3,1]
Explanation: The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.
```

**Example 2**

```
Input: nums = [0]
Output: [0]
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer.SingleSeqInPlacePointers;

/**
 * @author zhengxingxing
 * @date 2025/03/07
 */
public class SortArrayByParity {

    public static int[] sortArrayByParity(int[] nums) {
        // Initialize two pointers - left starts from beginning, right from end
        int left = 0;
        int right = nums.length - 1;

        // Continue until left and right pointers meet
        while (left < right) {
            // Move left pointer until we find an odd number
            while (left < right && nums[left] % 2 == 0) {
                left++;
            }

            // Move right pointer until we find an even number
            while (left < right && nums[right] % 2 == 1) {
                right--;
            }

            // Swap the odd number from left with even number from right
            if (left < right) {
                int temp = nums[left];
                nums[left] = nums[right];
                nums[right] = temp;
            }
        }
        return nums;
    }

    private static void printArray(int[] nums) {
        System.out.print("[");
        for (int i = 0; i < nums.length; i++) {
            System.out.print(nums[i]);
            if (i < nums.length - 1) {
                System.out.print(", ");
            }
        }
        System.out.println("]");
    }
    
    public static void main(String[] args) {
        // Test Case 1: Mixed even and odd numbers
        int[] nums1 = {3, 1, 2, 4};
        System.out.println("Test Case 1:");
        System.out.print("Original Array: ");
        printArray(nums1);
        int[] result1 = sortArrayByParity(nums1);
        System.out.print("Sorted Array: ");
        printArray(result1);
        System.out.println();

        // Test Case 2: Single element array
        int[] nums2 = {0};
        System.out.println("Test Case 2:");
        System.out.print("Original Array: ");
        printArray(nums2);
        int[] result2 = sortArrayByParity(nums2);
        System.out.print("Sorted Array: ");
        printArray(result2);
        System.out.println();

        // Test Case 3: Array with all odd numbers
        int[] nums3 = {1, 3, 5, 7};
        System.out.println("Test Case 3:");
        System.out.print("Original Array: ");
        printArray(nums3);
        int[] result3 = sortArrayByParity(nums3);
        System.out.print("Sorted Array: ");
        printArray(result3);
        System.out.println();

        // Test Case 4: Array with all even numbers
        int[] nums4 = {2, 4, 6, 8};
        System.out.println("Test Case 4:");
        System.out.print("Original Array: ");
        printArray(nums4);
        int[] result4 = sortArrayByParity(nums4);
        System.out.print("Sorted Array: ");
        printArray(result4);
    }
}

```





