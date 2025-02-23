---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "611. Valid Triangle Number"
date: "2025-02-23"
tags: Medium
categories:
  - "LeetCode TwoPointers"
---

- Given an integer array `nums`, return *the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle*.


**Example 1**

```
Input: nums = [2,2,3,4]
Output: 3
Explanation: Valid combinations are: 
2,3,4 (using the first 2)
2,3,4 (using the second 2)
2,2,3
```

**Example 2**

```
Input: nums = [4,2,3,4]
Output: 4
```

## Method 1

```tex
【O(n²) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/02/23
 */
public class ValidTriangleNumber {

    public static int triangleNumber(int[] nums) {
        // Handle edge cases - null array or insufficient elements
        if (nums == null || nums.length < 3) {
            return 0;
        }

        // Sort array to optimize the search process
        Arrays.sort(nums);
        int count = 0;
        int n = nums.length;

        // Iterate from largest possible side (right to left)
        for (int i = n - 1; i >= 2; i--) {
            // Initialize two pointers for remaining two sides
            int left = 0;          // Pointer for smallest side
            int right = i - 1;     // Pointer for middle side

            // Use two pointer technique to find valid combinations
            while (left < right) {
                // Check if current combination forms valid triangle
                if (nums[left] + nums[right] > nums[i]) {
                    // All numbers between left and right will also form valid triangles
                    // Because array is sorted and we're starting from largest side
                    count += right - left;
                    // Decrease right pointer to try smaller middle side
                    right--;
                } else {
                    // Sum is too small, need to increase left pointer
                    left++;
                }
            }
        }
        return count;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Basic case with duplicate elements
        int[] nums1 = {2, 2, 3, 4};
        System.out.println("Test Case 1:");
        System.out.println("Input: nums = [2,2,3,4]");
        System.out.println("Output: " + triangleNumber(nums1));
        System.out.println("Expected: 3");  // Valid combinations: (2,2,3), (2,3,4), (2,3,4)
        System.out.println();

        // Test Case 2: Array with duplicate maximum values
        int[] nums2 = {4, 2, 3, 4};
        System.out.println("Test Case 2:");
        System.out.println("Input: nums = [4,2,3,4]");
        System.out.println("Output: " + triangleNumber(nums2));
        System.out.println("Expected: 4");  // Valid combinations: (2,3,4), (2,4,4), (3,4,4)
        System.out.println();
    }
}

```





