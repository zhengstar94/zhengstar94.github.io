---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3105. Longest Strictly Increasing or Strictly Decreasing Subarray"
date: "2025-02-03"
tags: Easy
categories:
  - "LeetCode Array"
---


- You are given an array of integers `nums`. Return *the length of the **longest** subarray of* `nums` *which is either **strictly increasing** or **strictly decreasing***.

**Example 1**

```
Input: nums = [1,4,3,3,2]

Output: 2

Explanation:

The strictly increasing subarrays of nums are [1], [2], [3], [3], [4], and [1,4].

The strictly decreasing subarrays of nums are [1], [2], [3], [3], [4], [3,2], and [4,3].

Hence, we return 2.
```

**Example 2**

```
Input: nums = [3,3,3,3]

Output: 1

Explanation:

The strictly increasing subarrays of nums are [3], [3], [3], and [3].

The strictly decreasing subarrays of nums are [3], [3], [3], and [3].

Hence, we return 1.
```

**Example 3**

```
Input: nums = [3,2,1]

Output: 3

Explanation:

The strictly increasing subarrays of nums are [3], [2], and [1].

The strictly decreasing subarrays of nums are [3], [2], [1], [3,2], [2,1], and [3,2,1].

Hence, we return 3.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/02/03
 */
public class LongestStrictlyIncreasingOrStrictlyDecreasingSubarray {
    
    public static int longestMonotonicSubarray(int[] nums) {
        // Handle edge cases
        if (nums == null || nums.length == 0){
            return 0;  // Return 0 for null or empty array
        }

        if (nums.length == 1){
            return 1;  // Return 1 for single element array
        }

        // Initialize variables:
        // inc: length of current increasing sequence
        // dec: length of current decreasing sequence
        // maxLen: maximum length found so far
        int inc = 1;
        int dec = 1;
        int maxLen = 1;

        // Iterate through the array starting from second element
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > nums[i - 1]){  // If current element is greater than previous
                inc = inc + 1;  // Extend increasing sequence
                dec = 1;        // Reset decreasing sequence
            }else if(nums[i] < nums[i - 1]){  // If current element is less than previous
                dec = dec + 1;  // Extend decreasing sequence
                inc = 1;        // Reset increasing sequence
            }else{  // If current element equals previous
                inc = 1;        // Reset both sequences
                dec = 1;
            }

            // Update maximum length found
            maxLen = Math.max(maxLen, Math.max(inc, dec));
        }

        return maxLen;  // Return the final result
    }

    /**
     * Main method to test the implementation with various test cases
     */
    public static void main(String[] args) {
        // Test Case 1: Mixed sequence
        int[] nums1 = {1,4,3,3,2};
        System.out.println("Test Case 1 Result: " + longestMonotonicSubarray(nums1)); // Expected output: 2

        // Test Case 2: Equal elements
        int[] nums2 = {3,3,3,3};
        System.out.println("Test Case 2 Result: " + longestMonotonicSubarray(nums2)); // Expected output: 1

        // Test Case 3: Strictly decreasing sequence
        int[] nums3 = {3,2,1};
        System.out.println("Test Case 3 Result: " + longestMonotonicSubarray(nums3)); // Expected output: 3

        // Test Case 4: Strictly increasing sequence
        int[] nums4 = {1,2,3,4,5};
        System.out.println("Test Case 4 Result: " + longestMonotonicSubarray(nums4)); // Expected output: 5

        // Test Case 5: Strictly decreasing sequence
        int[] nums5 = {5,4,3,2,1};
        System.out.println("Test Case 5 Result: " + longestMonotonicSubarray(nums5)); // Expected output: 5
    }
}

```





