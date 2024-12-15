---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "658. Find K Closest Elements"
date: "2024-12-15"
tags: Medium
categories:
  - "LeetCode Binary Search"
---

- Given a **sorted** integer array `arr`, two integers `k` and `x`, return the `k` closest integers to `x` in the array. The result should also be sorted in ascending order.
- An integer `a` is closer to `x` than an integer `b` if:
  - `|a - x| < |b - x|`, or
  - `|a - x| == |b - x|` and `a < b`

**Example 1**

```
Input: arr = [1,2,3,4,5], k = 4, x = 3

Output: [1,2,3,4]
```

**Example 2**

```
Input: arr = [1,1,2,3,4,5], k = 4, x = -1

Output: [1,1,2,3]
```

## Method 1

```tex
【O(k) time | O(k) space】
```

```java
package Leetcode.BinarySearch;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2024/12/15
 */
public class FindKClosestElements {
    
    public static List<Integer> findClosestElements(int[] arr, int k, int x) {
        int left = 0;
        int right = arr.length - 1;

        // Loop until the subarray has exactly k elements
        while (right - left >= k) {
            // Compare the distances between arr[left] and arr[right] to x
            if (Math.abs(arr[left] - x) <= Math.abs(arr[right] - x)) {
                // If the left element is closer or equal to x, move the right pointer left
                right--;
            } else {
                // Otherwise, move the left pointer right
                left++;
            }
        }

        // Create a list to store the k closest elements
        List<Integer> result = new ArrayList<>();
        // Add the elements between left and right pointers to the result
        for (int i = left; i <= right; i++) {
            result.add(arr[i]);
        }
        return result;
    }

    public static void main(String[] args) {

        // Test case 1
        int[] arr1 = {1, 2, 3, 4, 5};
        int k1 = 4;
        int x1 = 3;
        System.out.println("Test Case 1: " + findClosestElements(arr1, k1, x1));
        // Expected output: [1, 2, 3, 4]

        // Test case 2
        int[] arr2 = {1, 1, 2, 3, 4, 5};
        int k2 = 4;
        int x2 = -1;
        System.out.println("Test Case 2: " + findClosestElements(arr2, k2, x2));
        // Expected output: [1, 1, 2, 3]

        // Test case 3
        int[] arr3 = {1, 3, 5, 7, 9};
        int k3 = 3;
        int x3 = 6;
        System.out.println("Test Case 3: " + findClosestElements(arr3, k3, x3));
        // Expected output: [3, 5, 7]

        // Test case 4
        int[] arr4 = {1, 5, 10, 15};
        int k4 = 2;
        int x4 = 12;
        System.out.println("Test Case 4: " + findClosestElements(arr4, k4, x4));
        // Expected output: [10, 15]

        // Test case 5
        int[] arr5 = {0, 1, 2, 3, 4, 5};
        int k5 = 3;
        int x5 = 2;
        System.out.println("Test Case 5: " + findClosestElements(arr5, k5, x5));
        // Expected output: [1, 2, 3]
    }
}

```





