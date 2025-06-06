---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "540. Single Element in a Sorted Array"
date: "2025-06-06"
tags: Medium
categories:
  - "LeetCode BinarySearch"
---


- You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.
- Return *the single element that appears only once*.
- Your solution must run in `O(log n)` time and `O(1)` space.

**Example 1**

```
Input: nums = [1,1,2,3,3,4,4,8,8]
Output: 2
```

**Example 2**

```
Input: nums = [3,3,7,7,10,11,11]
Output: 10
```

## Method 1

```tex
【O(log(n)) time | O(1) space】
```

```java
package Leetcode.BinarySearch;

/**
 * @Author zhengxingxing
 * @Date 2025/06/06
 */
public class SingleElementInASortedArray {
    
    public static int singleNonDuplicate(int[] nums) {
        // Initialize binary search boundaries
        int left = 0;                    // Start of search range
        int right = nums.length - 1;     // End of search range

        // Continue binary search until we narrow down to a single element
        while (left < right) {
            // Calculate middle index, avoiding integer overflow
            int mid = left + (right - left) / 2;

            /**
             * CRITICAL STEP: Ensure mid is at an even index
             *
             * Why this matters:
             * - In the portion before the single element, pairs start at even indices: (0,1), (2,3), (4,5)...
             * - In the portion after the single element, pairs start at odd indices due to shift
             *
             * By always checking from an even index, we can reliably determine:
             * - If nums[even] == nums[even+1]: we're in the "before single element" portion
             * - If nums[even] != nums[even+1]: we're in the "after single element" portion or at the single element
             */
            if (mid % 2 == 1) {
                mid--;  // Move to the previous even index
            }

            /**
             * Decision Logic:
             *
             * Case 1: nums[mid] == nums[mid + 1]
             * - This means we found a proper pair starting at an even index
             * - This indicates we're in the "before single element" portion
             * - The single element must be to the RIGHT of this pair
             * - Move left boundary to mid + 2 (skip this pair entirely)
             *
             * Case 2: nums[mid] != nums[mid + 1]
             * - This means the pair pattern is broken
             * - Either mid is the single element, or we're in the "after single element" portion
             * - The single element must be at mid or to the LEFT of mid
             * - Move right boundary to mid (include mid in next search)
             */
            if (nums[mid] == nums[mid + 1]) {
                // Single element is in the right half
                left = mid + 2;  // Skip the current pair and search right
            } else {
                // Single element is in the left half (including current position)
                right = mid;     // Include current position in next search
            }
        }

        // When left == right, we've found the single element
        return nums[left];
    }


    public static void main(String[] args) {
        // Test Case 1: Single element in the middle
        // Array structure: [1,1] [2] [3,3] [4,4] [8,8]
        // Expected: 2 (breaks the pair pattern)
        int[] nums1 = {1, 1, 2, 3, 3, 4, 4, 8, 8};
        System.out.println("Test Case 1:");
        System.out.println("Input: " + java.util.Arrays.toString(nums1));
        System.out.println("Output: " + singleNonDuplicate(nums1));
        System.out.println("Expected: 2");
        System.out.println();

        // Test Case 2: Single element towards the end
        // Array structure: [3,3] [7,7] [10] [11,11]
        // Expected: 10
        int[] nums2 = {3, 3, 7, 7, 10, 11, 11};
        System.out.println("Test Case 2:");
        System.out.println("Input: " + java.util.Arrays.toString(nums2));
        System.out.println("Output: " + singleNonDuplicate(nums2));
        System.out.println("Expected: 10");
        System.out.println();

        // Test Case 3: Single element at the beginning
        // Array structure: [1] [2,2] [3,3] [4,4] [8,8]
        // Expected: 1 (all subsequent pairs shift to odd start indices)
        int[] nums3 = {1, 2, 2, 3, 3, 4, 4, 8, 8};
        System.out.println("Test Case 3 (Single element at beginning):");
        System.out.println("Input: " + java.util.Arrays.toString(nums3));
        System.out.println("Output: " + singleNonDuplicate(nums3));
        System.out.println("Expected: 1");
        System.out.println();

        // Test Case 4: Single element at the end
        // Array structure: [1,1] [2,2] [3,3] [4,4] [8]
        // Expected: 8 (last element has no pair)
        int[] nums4 = {1, 1, 2, 2, 3, 3, 4, 4, 8};
        System.out.println("Test Case 4 (Single element at end):");
        System.out.println("Input: " + java.util.Arrays.toString(nums4));
        System.out.println("Output: " + singleNonDuplicate(nums4));
        System.out.println("Expected: 8");
        System.out.println();

        // Test Case 5: Edge case - single element array
        // Array structure: [1]
        // Expected: 1 (only one element exists)
        int[] nums5 = {1};
        System.out.println("Test Case 5 (Single element array):");
        System.out.println("Input: " + java.util.Arrays.toString(nums5));
        System.out.println("Output: " + singleNonDuplicate(nums5));
        System.out.println("Expected: 1");
    }
}


```





