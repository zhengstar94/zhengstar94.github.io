---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "845. Longest Mountain in Array"
date: "2025-04-14"
tags: Medium
categories:
  - "LeetCode GroupedLoop"
---

- You may recall that an array `arr` is a **mountain array** if and only if:
- `arr.length >= 3`
- There exists some index `i` (**0-indexed**) with `0 < i < arr.length - 1` such that:
  - `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]``
  - ``arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`
- Given an integer array `arr`, return *the length of the longest subarray, which is a mountain*. Return `0` if there is no mountain subarray.

**Example 1**

```
Input: arr = [2,1,4,7,3,2,5]
Output: 5
Explanation: The largest mountain is [1,4,7,3,2] which has length 5.
```

**Example 2**

```
Input: arr = [2,2,2]
Output: 0
Explanation: There is no mountain.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

/**
 * @author zhengxingxing
 * @date 2025/04/14
 */
public class LongestMountainInArray {

    public static int longestMountain(int[] arr) {
        // Variable to store the maximum length of mountain found
        int ans = 0;
        // Length of the input array
        int n = arr.length;
        // Current position in the array
        int i = 0;

        // Main loop: continues until we can't form a mountain (needs at least 3 elements)
        while (i < n - 2) {
            // Skip non-mountain starts: move forward until we find a potential mountain start
            // A mountain start is where the next element is smaller than current element
            while (i < n - 1 && arr[i] >= arr[i + 1]) {
                i++;
            }

            // Mark the start position of potential mountain
            int start = i;

            // Find the ascending sequence of the mountain
            // Continue moving forward while elements are strictly increasing
            while (i < n - 1 && arr[i] < arr[i + 1]) {
                i++;
            }

            // If no ascending sequence found, skip this position and continue
            // This happens when i hasn't moved from start position
            if (i == start) {
                i++;
                continue;
            }

            // Mark the peak position
            int peak = i;

            // Find the descending sequence of the mountain
            // Continue moving forward while elements are strictly decreasing
            while (i < n - 1 && arr[i] > arr[i + 1]) {
                i++;
            }

            // If we found both ascending and descending sequences (i moved past peak)
            // Calculate the length of this mountain and update maximum length if necessary
            if (i > peak) {
                ans = Math.max(ans, i - start + 1);
            }
        }

        // Return the length of the longest mountain found
        return ans;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Standard mountain array
        // Expected output: 5 (subarray [1,4,7,3,2] forms a mountain)
        int[] arr1 = {2,1,4,7,3,2,5};
        System.out.println("Test Case 1 Result: " + longestMountain(arr1));

        // Test Case 2: No mountain exists
        // Expected output: 0 (no increasing then decreasing sequence)
        int[] arr2 = {2,2,2};
        System.out.println("Test Case 2 Result: " + longestMountain(arr2));

        // Test Case 3: Multiple mountains
        // Expected output: 5 (largest mountain length among multiple mountains)
        int[] arr3 = {1,2,3,2,1,4,5,2,1};
        System.out.println("Test Case 3 Result: " + longestMountain(arr3));
    }
}

```





