---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2529. Maximum Count of Positive Integer and Negative Integer"
date: "2025-04-23"
tags: Easy
categories:
  - "LeetCode Binary Search"
---


- Given an array `nums` sorted in **non-decreasing** order, return *the maximum between the number of positive integers and the number of negative integers.*
  - In other words, if the number of positive integers in `nums` is `pos` and the number of negative integers is `neg`, then return the maximum of `pos` and `neg`.
- **Note** that `0` is neither positive nor negative.

**Example 1**

```
Input: nums = [-2,-1,-1,1,2,3]
Output: 3
Explanation: There are 3 positive integers and 3 negative integers. The maximum count among them is 3.
```

**Example 2**

```
Input: nums = [-3,-2,-1,0,0,1,2]
Output: 3
Explanation: There are 2 positive integers and 3 negative integers. The maximum count among them is 3.
```

**Example 3**

```
Input: nums = [5,20,66,1314]
Output: 4
Explanation: There are 4 positive integers and 0 negative integers. The maximum count among them is 4.
```

## Method 1

```tex
【O(log(n)) time | O(1) space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengxingxing
 * @date 2025/04/23
 */
public class MaximumCountOfPositiveIntegerAndNegativeInteger {
    
    public static int maximumCount(int[] nums) {
        // Handle edge cases: empty array or null input
        if (nums == null || nums.length == 0) {
            return 0;
        }

        // Find the position of first non-negative number (>= 0) using binary search
        int firstNonNeg = binarySearch(nums, 0);
        // Find the position of first positive number (> 0) using binary search
        int firstPos = binarySearch(nums, 1);

        // Calculate counts:
        // Number of negative integers = position of first non-negative
        // Number of positive integers = total length - position of first positive
        int negCount = firstNonNeg;
        int posCount = nums.length - firstPos;

        // Return the maximum of the two counts
        return Math.max(negCount, posCount);
    }

    private static int binarySearch(int[] nums, int target) {
        // Initialize two pointers for binary search
        int left = 0;
        int right = nums.length;

        // Standard binary search loop
        while (left < right) {
            // Calculate middle point avoiding integer overflow
            int mid = left + (right - left) / 2;
            // If middle element is less than target, search in right half
            if (nums[mid] < target) {
                left = mid + 1;
            }
            // If middle element is >= target, search in left half
            else {
                right = mid;
            }
        }

        // Return the position where element should be inserted
        return left;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Equal number of positive and negative integers
        int[] nums1 = {-2,-1,-1,1,2,3};
        System.out.println("Test Case 1 (Expected 3): " + maximumCount(nums1));

        // Test Case 2: Array with zeros and unequal positive/negative counts
        int[] nums2 = {-3,-2,-1,0,0,1,2};
        System.out.println("Test Case 2 (Expected 3): " + maximumCount(nums2));

        // Test Case 3: Array with only positive integers
        int[] nums3 = {5,20,66,1314};
        System.out.println("Test Case 3 (Expected 4): " + maximumCount(nums3));

        // Test Case 4: Array with only zeros
        int[] nums4 = {0,0,0};
        System.out.println("Test Case 4 (Expected 0): " + maximumCount(nums4));

        // Test Case 5: Array with only negative integers
        int[] nums5 = {-5,-4,-3,-2,-1};
        System.out.println("Test Case 5 (Expected 5): " + maximumCount(nums5));
    }
}

```





