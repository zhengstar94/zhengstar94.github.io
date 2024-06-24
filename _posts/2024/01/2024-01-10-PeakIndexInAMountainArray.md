---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "852. Peak Index in a Mountain Array"
date: "2024-01-10"
categories:
  - "LeetCode Binary Search"
---

# LeetCode 852. Peak Index in a Mountain Array [Medium]

- Let’s call an array `A` a *mountain* if the following properties hold:
  - `A.length >= 3`
  - There exists some `0 < i < A.length - 1` such that `A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]`
- Given an array that is definitely a mountain, return any `i` such that `A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]`.

**Example 1**

```
Input: [0,1,0]
Output: 1
```

**Example 2**

```
Input: [0,2,1,0]
Output: 1
```

## Method 1

```tex
【O(log(n))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2024/02/13
 */
public class PeakIndexInAMountainArray {
    public static int peakIndexInMountainArray(int[] arr) {
        // initialize the left bound of the search range
        int left = 0;  
        // initialize the right bound of the search range
        int right = arr.length - 1;  

        // Continue to do binary search until left < right
        while (left < right) {
            // calculate the mid index
            int mid = left + (right - left) / 2;  

            // if arr[mid] < arr[mid + 1], the peak must be in the right half.
            if (arr[mid] < arr[mid + 1]) {
                // update the left bound
                left = mid + 1;  
            } else {
                // If arr[mid] > arr[mid + 1], the peak must be in the left half
                // update the right bound
                right = mid;  
            }
        }

        // return the index of the peak
        return left;
    }

    public static void main(String[] args) {
        // Test case 1: a regular example
        int[] arr1 = {0, 1, 0};
        System.out.println(peakIndexInMountainArray(arr1));  // Expected output: 1

        // Test case 2: a large example
        int[] arr2 = {0, 2, 3, 5, 4, 2, 1};
        System.out.println(peakIndexInMountainArray(arr2));  // Expected output: 3

        // Test case 3: another regular example
        int[] arr3 = {0, 10, 5, 2};
        System.out.println(peakIndexInMountainArray(arr3));  // Expected output: 1
    }
}

```

