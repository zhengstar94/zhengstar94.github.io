---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1299. Replace Elements with Greatest Element on Right Side"
date: "2025-02-16"
tags: Easy
categories:
  - "LeetCode Array"
---


- Given an array `arr`, replace every element in that array with the greatest element among the elements to its right, and replace the last element with `-1`.
- After doing so, return the array.


**Example 1**

```
Input: arr = [17,18,5,4,6,1]
Output: [18,6,6,6,1,-1]
Explanation: 
- index 0 --> the greatest element to the right of index 0 is index 1 (18).
- index 1 --> the greatest element to the right of index 1 is index 4 (6).
- index 2 --> the greatest element to the right of index 2 is index 4 (6).
- index 3 --> the greatest element to the right of index 3 is index 4 (6).
- index 4 --> the greatest element to the right of index 4 is index 5 (1).
- index 5 --> there are no elements to the right of index 5, so we put -1.
```

**Example 2**

```
Input: arr = [400]
Output: [-1]
Explanation: There are no elements to the right of index 0.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/02/16
 */
public class ReplaceElementsWithGreatestElementOnRightSide {
    
    public static int[] replaceElements(int[] arr) {
        int n = arr.length;  // Get the length of input array
        int max = -1;        // Initialize max as -1 (will be the value for the last element)

        // Traverse array from right to left
        for (int i = n - 1; i >= 0; i--) {
            int temp = arr[i];    // Store current element temporarily
            arr[i] = max;         // Replace current element with maximum value seen so far
            max = Math.max(max, temp);  // Update maximum value
        }

        return arr;
    }

    
    public static void main(String[] args) {
        // Test Case 1: Array with multiple elements
        int[] arr1 = {17, 18, 5, 4, 6, 1};
        int[] result1 = replaceElements(arr1);
        System.out.println(Arrays.toString(result1)); // Expected output: [18, 6, 6, 6, 1, -1]

        // Test Case 2: Array with single element
        int[] arr2 = {400};
        int[] result2 = replaceElements(arr2);
        System.out.println(Arrays.toString(result2)); // Expected output: [-1]

        // Test Case 3: Array with decreasing sequence
        int[] arr = {5, 4, 3, 2, 1};
        int[] result = replaceElements(arr);
        System.out.println(Arrays.toString(result)); // Expected output: [4, 3, 2, 1, -1]
    }
}

```





