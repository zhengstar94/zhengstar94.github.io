---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1287. Element Appearing More Than 25% In Sorted Array"
date: "2025-02-17"
tags: Easy
categories:
  - "LeetCode Array"
---

- Given an integer array **sorted** in non-decreasing order, there is exactly one integer in the array that occurs more than 25% of the time, return that integer.


**Example 1**

```
Input: arr = [1,2,2,6,6,6,6,7,10]
Output: 6
```

**Example 2**

```
Input: arr = [1,1]
Output: 1
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/02/17
 */
public class ElementAppearingMoreThan25PercentInSortedArray {

    public static int findSpecialInteger(int[] arr) {
        // Get the length of input array
        int n = arr.length;

        // Calculate quarter of array length
        // This is our window size as element must appear more than 25%
        int quarter = n / 4;

        // Iterate through array until (n - quarter)
        // This ensures we don't get ArrayIndexOutOfBounds when checking arr[i + quarter]
        for (int i = 0; i < n - quarter; i++) {
            // If current element equals element at quarter distance ahead
            // We found our answer as it spans more than 25% of array
            if (arr[i] == arr[i + quarter]) {
                return arr[i];
            }
        }

        // Return last element for special cases:
        // 1. When array length is very small (≤ 4)
        // 2. When all elements are the same
        // This works because array is sorted and answer must exist
        return arr[n-1];
    }

    
    public static void main(String[] args) {
        // Test case 1: Element appears in middle of array
        // Expected output: 6 (appears 4 times, which is more than 25% of 9)
        int[] input1 = {1,2,2,6,6,6,6,7,10};
        System.out.println(findSpecialInteger(input1));

        // Test case 2: Minimal array with two same elements
        // Expected output: 1 (appears 2 times, which is 100% of array)
        int[] input2 = {1,1};
        System.out.println(findSpecialInteger(input2));

        // Test case 3: Element appears at the beginning
        // Expected output: 2 (appears 4 times, which is more than 25% of 7)
        int[] input3 = {2,2,2,2,3,4,5};
        System.out.println(findSpecialInteger(input3));
    }
}

```





