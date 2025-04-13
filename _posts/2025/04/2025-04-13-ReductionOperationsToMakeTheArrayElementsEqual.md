---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1887. Reduction Operations to Make the Array Elements Equal"
date: "2025-04-13"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---


- Given an integer array `nums`, your goal is to make all elements in `nums` equal. To complete one operation, follow these steps:
  1. Find the **largest** value in `nums`. Let its index be `i` (**0-indexed**) and its value be `largest`. If there are multiple elements with the largest value, pick the smallest `i`.
  2. Find the **next largest** value in `nums` **strictly smaller** than `largest`. Let its value be `nextLargest`.
  3. Reduce `nums[i]` to `nextLargest`.
- Return *the number of operations to make all elements in* `nums` *equal*.

**Example 1**

```
Input: nums = [5,1,3]
Output: 3
Explanation: It takes 3 operations to make all elements in nums equal:
1. largest = 5 at index 0. nextLargest = 3. Reduce nums[0] to 3. nums = [3,1,3].
2. largest = 3 at index 0. nextLargest = 1. Reduce nums[0] to 1. nums = [1,1,3].
3. largest = 3 at index 2. nextLargest = 1. Reduce nums[2] to 1. nums = [1,1,1].
```

**Example 2**

```
Input: nums = [1,1,1]
Output: 0
Explanation: All elements in nums are already equal.
```

**Example 3**

```
Input: nums = [1,1,2,2,3]
Output: 4
Explanation: It takes 4 operations to make all elements in nums equal:
1. largest = 3 at index 4. nextLargest = 2. Reduce nums[4] to 2. nums = [1,1,2,2,2].
2. largest = 2 at index 2. nextLargest = 1. Reduce nums[2] to 1. nums = [1,1,1,2,2].
3. largest = 2 at index 3. nextLargest = 1. Reduce nums[3] to 1. nums = [1,1,1,1,2].
4. largest = 2 at index 4. nextLargest = 1. Reduce nums[4] to 1. nums = [1,1,1,1,1].
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/04/13
 */
public class ReductionOperationsToMakeTheArrayElementsEqual {
    public static int reductionOperations(int[] nums) {
        // Sort the array to group identical elements together
        // This makes it easier to process elements from largest to smallest
        Arrays.sort(nums);

        // Initialize variables
        int n = nums.length;              // Length of the array
        int i = n - 1;                    // Start from the last element (largest value)
        int result = 0;                   // Counter for total operations needed

        // Main loop continues while we have elements to process
        // We need at least two different values to perform operations
        while (i > 0){
            // Store the starting index of current group
            int start = i;

            // Inner loop: Find all elements with the same value
            // This loop groups identical elements together
            while (i > 0 && nums[i] == nums[i - 1]){
                // Move backward while we find same values
                // This helps us identify the size of current group
                i--;
            }

            // Process current group if it's not the smallest value group
            if (i > 0){
                // Calculate operations needed for current group
                // n - i represents the number of elements that need to be reduced
                // For each element larger than the next distinct value,
                // we need one operation to reduce it
                result += n - i;
            }

            // Move to the next group
            // We decrement i here because the current position has been processed
            i--;
        }

        return result;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Array with distinct elements
        int[] nums1 = {5, 1, 3};
        System.out.println("Test Case 1 Result: " + reductionOperations(nums1));
        // Expected output: 3 (requires 3 operations to make all elements equal)

        // Test Case 2: Array with all equal elements
        int[] nums2 = {1, 1, 1};
        System.out.println("Test Case 2 Result: " + reductionOperations(nums2));
        // Expected output: 0 (no operations needed as elements are already equal)

        // Test Case 3: Array with mixed repeated elements
        int[] nums3 = {1, 1, 2, 2, 3};
        System.out.println("Test Case 3 Result: " + reductionOperations(nums3));
        // Expected output: 4 (requires 4 operations to make all elements equal)
    }
}

```





