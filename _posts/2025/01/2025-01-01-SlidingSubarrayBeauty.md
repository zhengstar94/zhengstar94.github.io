---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2653. Sliding Subarray Beauty"
date: "2025-01-01"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---


- Given an integer array `nums` containing `n` integers, find the **beauty** of each subarray of size `k`.
- The **beauty** of a subarray is the `xth` **smallest integer** in the subarray if it is **negative**, or `0` if there are fewer than `x` negative integers.
- Return *an integer array containing* `n - k + 1` *integers, which denote the* **beauty** *of the subarrays **in order** from the first index in the array.*
  - A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1**

```
Input: nums = [1,-1,-3,-2,3], k = 3, x = 2
Output: [-1,-2,-2]
Explanation: There are 3 subarrays with size k = 3. 
The first subarray is [1, -1, -3] and the 2nd smallest negative integer is -1. 
The second subarray is [-1, -3, -2] and the 2nd smallest negative integer is -2. 
The third subarray is [-3, -2, 3] and the 2nd smallest negative integer is -2.
```

**Example 2**

```
Input: nums = [-1,-2,-3,-4,-5], k = 2, x = 2
Output: [-1,-2,-3,-4]
Explanation: There are 4 subarrays with size k = 2.
For [-1, -2], the 2nd smallest negative integer is -1.
For [-2, -3], the 2nd smallest negative integer is -2.
For [-3, -4], the 2nd smallest negative integer is -3.
For [-4, -5], the 2nd smallest negative integer is -4. 
```

**Example 3**

```
Input: nums = [-3,1,2,-3,0,-3], k = 2, x = 1
Output: [-3,0,-3,-3,-3]
Explanation: There are 5 subarrays with size k = 2.
For [-3, 1], the 1st smallest negative integer is -3.
For [1, 2], there is no negative integer so the beauty is 0.
For [2, -3], the 1st smallest negative integer is -3.
For [-3, 0], the 1st smallest negative integer is -3.
For [0, -3], the 1st smallest negative integer is -3.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/01
 */
public class SlidingSubarrayBeauty {
    public static int[] getSubarrayBeauty(int[] nums, int k, int x) {
        int n = nums.length;
        int[] result = new int[n - k + 1];

        // Use a counting array to track the frequency of negative numbers
        // Since array elements are limited to [-50, 50], we only need to count negatives
        // Index i in count array represents number -i
        int[] count = new int[51]; // indices 0-50 represent numbers -50 to 0

        // Initialize the first window by counting negative numbers
        for (int i = 0; i < k; i++) {
            if (nums[i] < 0) {
                count[-nums[i]]++; // Convert negative number to positive index
            }
        }

        // Process the first window result
        result[0] = findXthNegative(count, x);

        // Slide the window and process subsequent windows
        for (int i = k; i < n; i++) {
            // Remove the leftmost number from the window
            // If it's negative, decrease its count
            if (nums[i - k] < 0) {
                count[-nums[i - k]]--;
            }

            // Add the rightmost number to the window
            // If it's negative, increase its count
            if (nums[i] < 0) {
                count[-nums[i]]++;
            }

            // Find the xth smallest negative number in current window
            result[i - k + 1] = findXthNegative(count, x);
        }

        return result;
    }

    /**
     * 在计数数组中找到第x小的负数
     * @param count 计数数组
     * @param x 要找的第x小的位置
     * @return 第x小的负数，如果负数个数少于x则返回0
     */
    private static int findXthNegative(int[] count, int x) {
        int sum = 0;
        // Iterate from -50 to -1 (represented as indices 50 to 1)
        for (int i = 50; i > 0; i--) {
            // Add up counts until we reach or exceed x
            sum += count[i];
            if (sum >= x) {
                // When sum >= x, we've found the xth smallest negative number
                // Return the negative of current index
                return -i;
            }
        }
        // Return 0 if there are fewer than x negative numbers
        return 0;
    }

    public static void main(String[] args) {
        // Test case 1: Mixed positive and negative numbers
        int[] nums1 = new int[]{-3,1,2,-3,0,-3};
        int k1 = 2;
        int x1 = 1;
        int[] result1 = getSubarrayBeauty(nums1, k1, x1);
        System.out.print("Test case 1 result: ");
        printArray(result1);

        // Test case 2: All negative numbers
        int[] nums2 = new int[]{-1,-2,-3,-4,-5};
        int k2 = 3;
        int x2 = 2;
        int[] result2 = getSubarrayBeauty(nums2, k2, x2);
        System.out.print("Test case 2 result: ");
        printArray(result2);

        // Test case 3: All positive numbers
        int[] nums3 = new int[]{1,2,3,4,5};
        int k3 = 2;
        int x3 = 1;
        int[] result3 = getSubarrayBeauty(nums3, k3, x3);
        System.out.print("Test case 3 result: ");
        printArray(result3);
    }

    // Helper method: Print array in formatted way
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





