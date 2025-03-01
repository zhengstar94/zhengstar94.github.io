---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1574. Shortest Subarray to be Removed to Make Array Sorted"
date: "2025-03-01"
tags: Medium TwoPointers
categories:
  - "LeetCode SingleSeqTwoPointersForward" 
---

- Given an integer array `arr`, remove a subarray (can be empty) from `arr` such that the remaining elements in `arr` are **non-decreasing**.
- Return *the length of the shortest subarray to remove*.
- A **subarray** is a contiguous subsequence of the array.

**Example 1**

```
Input: arr = [1,2,3,10,4,2,3,5]
Output: 3
Explanation: The shortest subarray we can remove is [10,4,2] of length 3. The remaining elements after that will be [1,2,3,3,5] which are sorted.
Another correct solution is to remove the subarray [3,10,4].
```

**Example 2**

```
Input: arr = [5,4,3,2,1]
Output: 4
Explanation: Since the array is strictly decreasing, we can only keep a single element. Therefore we need to remove a subarray of length 4, either [5,4,3,2] or [4,3,2,1].
```

**Example 3**

```
Input: arr = [1,2,3]
Output: 0
Explanation: The array is already non-decreasing. We do not need to remove any elements.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer.SingleSeqTwoPointersForward;

/**
 * @author zhengxingxing
 * @date 2025/03/01
 */
public class ShortestSubarrayToBeRemovedToMakeArraySorted {
    public static int findLengthOfShortestSubarray(int[] arr) {
        int n = arr.length;

        // Find the end position of the left increasing sequence
        // Keep moving right while the array is increasing
        int left = 0;
        while (left + 1 < n && arr[left] <= arr[left + 1]) {
            left++;
        }

        // If the entire array is already sorted (increasing)
        // then we don't need to remove any elements
        if (left == n - 1) {
            return 0;
        }

        // Find the start position of the right increasing sequence
        // Keep moving left while the array is increasing from the right
        int right = n - 1;
        while (right > 0 && arr[right - 1] <= arr[right]) {
            right--;
        }

        // Initialize the minimum length to remove
        // Two basic options:
        // 1. n - left - 1: remove elements after the left increasing sequence
        // 2. right: remove elements before the right increasing sequence
        // The -1 in (n - left - 1) is because we want to keep the last element of left sequence
        int result = Math.min(n - left - 1, right);

        // Use two pointers to try to merge left and right increasing sequences
        // i: points to elements in left increasing sequence
        // j: points to elements in right increasing sequence
        int i = 0, j = right;
        while (i <= left && j < n) {
            if (arr[i] <= arr[j]) {
                // Current combination is valid because arr[i] <= arr[j]
                // This means we can keep elements from [0...i] and [j...n-1]
                // Need to remove elements between i and j, which is (j-i-1) elements
                // Update result if this removal length is smaller
                result = Math.min(result, j - i - 1);
                i++; // Try next element from left sequence
            } else {
                // Current arr[i] > arr[j], which breaks increasing order
                // Move j to find a larger element in right sequence
                j++;
            }
        }

        return result;
    }

    // Test cases to verify the solution
    public static void main(String[] args) {
        // Test case 1: Array with middle portion to be removed
        // Expected result: 3 (can remove [10,4,2] or [3,10,4])
        int[] arr1 = {1,2,3,10,4,2,3,5};
        System.out.println("Test case 1 result: " + findLengthOfShortestSubarray(arr1));

        // Test case 2: Strictly decreasing array
        // Expected result: 4 (need to remove all but one element)
        int[] arr2 = {5,4,3,2,1};
        System.out.println("Test case 2 result: " + findLengthOfShortestSubarray(arr2));

        // Test case 3: Already sorted array
        // Expected result: 0 (no need to remove any elements)
        int[] arr3 = {1,2,3};
        System.out.println("Test case 3 result: " + findLengthOfShortestSubarray(arr3));
    }
}

```





