---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "977.Squares of a Sorted Array"
date: "2024-02-12"
categories:
  - "LeetCode Two Pointers"
---

# LeetCode 977. Squares of a Sorted Array [Easy]

- Given an integer array `nums` sorted in **non-decreasing** order, return *an array of **the squares of each number** sorted in non-decreasing order*.

**Example 1**

```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```

**Example 2**

```
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.TwoPointer;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/02/14
 */
public class SquaresOfASortedArray {
    public static int[] sortedSquares(int[] nums) {
        // Compute the length of the given array
        int n = nums.length;
        // Create a new array to store the result
        int[] result = new int[n];
        // Initialize two pointers
        // The left pointer starts at the beginning of the array
        int left = 0;
        // The right pointer starts at the end of the array
        int right = n - 1;
        // Loop until all elements in the array are handled
        for (int i = n - 1; i >= 0; i--) {
            // If the absolute value of the number at the left pointer is less than the one at the right pointer
            if (Math.abs(nums[left]) < Math.abs(nums[right])) {
                // Square the number at the right pointer and store it into the result array
                result[i] = nums[right] * nums[right];
                // Move the right pointer to the left
                right--;
                // Otherwise
            } else {
                // Square the number at the left pointer and store it into the result array
                result[i] = nums[left] * nums[left];
                // Move the left pointer to the right
                left++;
            }
        }
        // Return the result array
        return result;
    }

    public static void main(String[] args) {

        // Test case 1: a regular example
        // After squaring, the array becomes [16,1,0,9,100]
        // After sorting, it becomes [0,1,9,16,100]
        System.out.println(Arrays.toString(sortedSquares(new int[] {-4,-1,0,3,10})));

        // Test case 2: another regular example
        // After squaring, the array becomes [49,9,4,9,121]
        // After sorting, it becomes [4,9,9,49,121]
        System.out.println(Arrays.toString(sortedSquares(new int[] {-7,-3,2,3,11})));
    }
}

```

