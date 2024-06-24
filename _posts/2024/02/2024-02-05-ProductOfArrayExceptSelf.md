---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "238.Product of Array Except Self"
date: "2024-02-05"
categories:
  - "LeetCode Array"
---

# LeetCode 238. Product of Array Except Self [Easy]

- Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.
- The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.
- You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1**

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

**Example 2**

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

## Method 1

```tex
【O(n)time∣O(1)space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/03/02
 */
public class ProductOfArrayExceptSelf {
    public static int[] productExceptSelf(int[] nums) {

        // Create a new result array with the same length as the input array
        int[] result = new int[nums.length];

        // Initialize all values in the result array to 1
        Arrays.fill(result,1);

        // Iterate over the nums array starting from the second value
        // Multiply the current result value by the previous nums value
        for(int i = 1; i < nums.length; i++){
            result[i] = result[i - 1] * nums[i - 1];
        }

        // Initialize a variable, right, to keep track of the product of values to the right of the current index
        int right = 1;

        // Iterate over the nums array in reverse order
        // Multiply the current result value by the current right value
        // Update right to be the product of its current value and the current nums value
        for(int j = nums.length - 1; j >=0 ; j--){
            result[j] = result[j] * right;
            right *= nums[j];
        }

        // Return the result array
        return result;
    }

    public static void main(String[] args) {
        // Test Case 1
        int[] nums1 = {1,2,3,4};
        // Print the result of the productExceptSelf method on nums1
        System.out.println(Arrays.toString(productExceptSelf(nums1)));  // Expected output: [24,12,8,6]

        // Test Case 2
        int[] nums2 = {-1,1,0,-3,3};
        // Print the result of the productExceptSelf method on nums2
        System.out.println(Arrays.toString(productExceptSelf(nums2)));  // Expected output: [0,0,9,0,0]
    }
}

```

