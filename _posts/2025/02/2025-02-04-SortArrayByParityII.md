---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "922. Sort Array By Parity II"
date: "2025-02-04"
tags: Easy TwoPointers
categories:
  - "LeetCode SingleSeqInPlacePointers"
---


- Given an array of integers `nums`, half of the integers in `nums` are **odd**, and the other half are **even**.
- Sort the array so that whenever `nums[i]` is odd, `i` is **odd**, and whenever `nums[i]` is even, `i` is **even**.
- Return *any answer array that satisfies this condition*.

**Example 1**

```
Input: nums = [4,2,5,7]
Output: [4,5,2,7]
Explanation: [4,7,2,5], [2,5,4,7], [2,7,4,5] would also have been accepted.
```

**Example 2**

```
Input: nums = [2,3]
Output: [2,3]
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/02/04
 */
public class SortArrayByParityII {
    
    public static int[] sortArrayByParityII(int[] nums) {
        // Initialize pointer for even indices starting from 0
        int evenIndex = 0;
        // Initialize pointer for odd indices starting from 1
        int oddIndex = 1;

        // Get the length of input array
        int n = nums.length;
        // Create a new array to store the result
        int[] result = new int[n];

        // Iterate through each number in the input array
        for (int num : nums) {
            if (num % 2 == 0) {
                // If number is even, place it at even index
                result[evenIndex] = num;
                // Move even index pointer by 2 positions
                evenIndex += 2;
            } else {
                // If number is odd, place it at odd index
                result[oddIndex] = num;
                // Move odd index pointer by 2 positions
                oddIndex += 2;
            }
        }
        // Return the sorted array
        return result;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Array with multiple elements
        int[] nums1 = {4, 2, 5, 7};
        int[] result1 = sortArrayByParityII(nums1);
        System.out.println("Test Case 1 Result: " + Arrays.toString(result1));

        // Test Case 2: Array with minimum elements
        int[] nums2 = {2, 3};
        int[] result2 = sortArrayByParityII(nums2);
        System.out.println("Test Case 2 Result: " + Arrays.toString(result2));
    }
}

```





