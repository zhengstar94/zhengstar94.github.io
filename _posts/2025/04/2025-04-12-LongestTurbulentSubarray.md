---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "978. Longest Turbulent Subarray"
date: "2025-04-12"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---


- Given an integer array `arr`, return *the length of a maximum size turbulent subarray of* `arr`.
- A subarray is **turbulent** if the comparison sign flips between each adjacent pair of elements in the subarray.
- More formally, a subarray `[arr[i], arr[i + 1], ..., arr[j]]` of `arr` is said to be turbulent if and only if:
  - For `i <= k < j`:
    - `arr[k] > arr[k + 1]` when `k` is odd, and
    - `arr[k] < arr[k + 1]` when `k` is even.
  - Or, for `i <= k < j`:
    - `arr[k] > arr[k + 1]` when `k` is even, and
    - `arr[k] < arr[k + 1]` when `k` is odd.

**Example 1**

```
Input: arr = [9,4,2,10,7,8,8,1,9]
Output: 5
Explanation: arr[1] > arr[2] < arr[3] > arr[4] < arr[5]
```

**Example 2**

```
Input: arr = [4,8,12,16]
Output: 2
```

**Example 3**

```
Input: arr = [100]
Output: 1
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/12
 */
public class LongestTurbulentSubarray {

    public static int maxTurbulenceSize(int[] arr) {
        // Handle edge cases: null array or empty array
        if (arr == null || arr.length == 0){
            return 0;
        }

        // Handle single element array
        if (arr.length == 1){
            return 1;
        }

        int n = arr.length;
        int maxLen = 1;  // Initialize max length to 1 (minimum possible length)
        int i = 0;  // Index to traverse the array

        // Outer loop: process each potential starting point of turbulent subarray
        while (i < n - 1){
            int start = i;  // Mark the start of current potential turbulent subarray

            // Skip adjacent equal elements as they break turbulence
            if (arr[i] == arr[i + 1]){
                i++;
                continue;
            }

            // Record the relationship between first pair of elements
            // This boolean will help maintain alternating pattern
            // true if current element is greater than next element
            // false if current element is less than next element
            boolean isFirstGreater = arr[i] > arr[i + 1];

            // Inner loop: extend the turbulent subarray as far as possible
            // Condition breakdown:
            // 1. i < n - 1: ensure we don't go out of bounds
            // 2. ((isFirstGreater && arr[i] > arr[i + 1]) || (!isFirstGreater && arr[i] < arr[i + 1]))
            //    - if isFirstGreater is true, we need current > next
            //    - if isFirstGreater is false, we need current < next
            //    This ensures alternating pattern
            while (i < n - 1 && ((isFirstGreater && arr[i] > arr[i + 1]) ||
                    (!isFirstGreater && arr[i] < arr[i + 1]))){
                // Flip the comparison sign for next pair
                isFirstGreater = !isFirstGreater;
                i++;  // Move to next element
            }

            // Calculate length of current turbulent subarray and update max length if necessary
            // i - start + 1 gives the length of current subarray
            maxLen = Math.max(maxLen, i - start + 1);
        }

        return maxLen;
    }

    public static void main(String[] args) {
        // Test case 1: Expected output 5
        // Turbulent subarray: [4,2,10,7,8] because 4>2<10>7<8
        int[] arr1 = {9,4,2,10,7,8,8,1,9};
        System.out.println("Test case 1 result: " + maxTurbulenceSize(arr1));

        // Test case 2: Expected output 2
        // All elements increasing, only adjacent pairs can be turbulent
        int[] arr2 = {4,8,12,16};
        System.out.println("Test case 2 result: " + maxTurbulenceSize(arr2));

        // Test case 3: Expected output 1
        // Single element array
        int[] arr3 = {100};
        System.out.println("Test case 3 result: " + maxTurbulenceSize(arr3));

        // Test case 4: Expected output 1
        // Equal adjacent elements
        int[] arr4 = {9,9};
        System.out.println("Test case 4 result: " + maxTurbulenceSize(arr4));
    }
}

```





