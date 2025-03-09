---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1385. Find the Distance Value Between Two Arrays"
date: "2025-03-09"
tags: Easy TwoPointers
categories:
  - "LeetCode DoubleSeqTwoPointers"
---

- Given two integer arrays `arr1` and `arr2`, and the integer `d`, *return the distance value between the two arrays*.
- The distance value is defined as the number of elements `arr1[i]` such that there is not any element `arr2[j]` where `|arr1[i]-arr2[j]| <= d`.

**Example 1**

```
Input: arr1 = [4,5,8], arr2 = [10,9,1,8], d = 2
Output: 2
Explanation: 
For arr1[0]=4 we have: 
|4-10|=6 > d=2 
|4-9|=5 > d=2 
|4-1|=3 > d=2 
|4-8|=4 > d=2 
For arr1[1]=5 we have: 
|5-10|=5 > d=2 
|5-9|=4 > d=2 
|5-1|=4 > d=2 
|5-8|=3 > d=2
For arr1[2]=8 we have:
|8-10|=2 <= d=2
|8-9|=1 <= d=2
|8-1|=7 > d=2
|8-8|=0 <= d=2
```

**Example 2**

```
Input: arr1 = [1,4,2,3], arr2 = [-4,-3,6,10,20,30], d = 3
Output: 2
```

**Example 3**

```
Input: arr1 = [2,1,100,3], arr2 = [-5,-2,10,-3,7], d = 6
Output: 1
```

## Method 1

```tex
【O(nlogn + mlogm) time | O(1) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqTwoPointers;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/03/09
 */
public class FindTheDistanceValueBetweenTwoArrays {

    public static int findTheDistanceValue(int[] arr1, int[] arr2, int d) {
        // Sort both arrays to enable efficient two-pointer traversal
        Arrays.sort(arr1);
        Arrays.sort(arr2);

        int ans = 0;  // Counter for valid elements
        int j = 0;    // Pointer for arr2

        // Iterate through each element in arr1
        for (int x : arr1) {
            // Move pointer j until we find first element in arr2 that could be within range
            // Skip all elements that are definitely too small (less than x-d)
            while (j < arr2.length && arr2[j] < x - d) {
                j++;
            }

            // If we've either:
            // 1. Reached the end of arr2, or
            // 2. The current element in arr2 is too large (greater than x+d)
            // Then x satisfies the distance condition
            if (j == arr2.length || arr2[j] > x + d) {
                ans++;
            }
        }

        return ans;
    }


    public static void main(String[] args) {
        // Test Case 1: Basic case with small arrays
        int[] arr1_1 = {4,5,8};
        int[] arr2_1 = {10,9,1,8};
        int d1 = 2;
        System.out.println("Test Case 1 Result: " +
                findTheDistanceValue(arr1_1, arr2_1, d1));  // Expected output: 2

        // Test Case 2: Arrays with negative numbers
        int[] arr1_2 = {1,4,2,3};
        int[] arr2_2 = {-4,-3,6,10,20,30};
        int d2 = 3;
        System.out.println("Test Case 2 Result: " +
                findTheDistanceValue(arr1_2, arr2_2, d2));  // Expected output: 2

        // Test Case 3: Array with large difference in values
        int[] arr1_3 = {2,1,100,3};
        int[] arr2_3 = {-5,-2,10,-3,7};
        int d3 = 6;
        System.out.println("Test Case 3 Result: " +
                findTheDistanceValue(arr1_3, arr2_3, d3));  // Expected output: 1
    }
}

```





