---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1343. Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold"
date: "2024-12-21"
tags: Medium SlideWindow
categories:
  - "LeetCode SlideWindow"
---


- Given an array of integers `arr` and two integers `k` and `threshold`, return *the number of sub-arrays of size* `k` *and average greater than or equal to* `threshold`.


**Example 1**

```
Input: arr = [2,2,2,2,5,5,5,8], k = 3, threshold = 4
Output: 3
Explanation: Sub-arrays [2,5,5],[5,5,5] and [5,5,8] have averages 4, 5 and 6 respectively. All other sub-arrays of size 3 have averages less than 4 (the threshold).
```

**Example 2**

```
Input: arr = [11,13,17,23,29,31,7,5,2,3], k = 3, threshold = 5
Output: 6
Explanation: The first 6 sub-arrays of size 3 have averages greater than 5. Note that averages are not integers.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2024/12/21
 */
public class NumberOfSubArraysOfSizeKAndAverageGreaterThanOrEqualToThreshold {
    public static int numOfSubarrays(int[] arr, int k, int threshold) {
        int n = arr.length;
        if(n == 0) {
            return 0;
        }

        // Running sum of current window
        int subArraySum = 0;
        // Count of valid subarrays
        int result = 0;

        // Iterate through the array
        for(int i = 0; i < n; i++) {
            // Add current element to window sum
            subArraySum += arr[i];

            // Skip until we have k elements in our window
            if(i < k - 1) {
                continue;
            }

            // Check if current window's average meets threshold
            if((subArraySum / k) >= threshold) {
                result++;
            }

            // Remove leftmost element from window sum
            // as we'll slide the window right in next iteration
            subArraySum -= arr[i - k + 1];
        }
        return result;
    }

    public static void main(String[] args) {

        // Test Case 1: Basic case
        System.out.println("Test Case 1:");
        int[] arr1 = {2,2,2,2,5,5,5,8};
        int k1 = 3;
        int threshold1 = 4;
        System.out.println("Expected: 3");
        System.out.println("Result: " + numOfSubarrays(arr1, k1, threshold1));

        // Test Case 2: No valid subarrays
        System.out.println("Test Case 2:");
        int[] arr2 = {1,1,1,1,1};
        int k2 = 2;
        int threshold2 = 2;
        System.out.println("Expected: 0");
        System.out.println("Result: " + numOfSubarrays(arr2, k2, threshold2));

        // Test Case 3: All subarrays valid
        System.out.println("Test Case 3:");
        int[] arr3 = {5,5,5,5};
        int k3 = 2;
        int threshold3 = 4;
        System.out.println("Expected: 3");
        System.out.println("Result: " + numOfSubarrays(arr3, k3, threshold3));

        // Test Case 4: Empty array
        System.out.println("Test Case 4:");
        int[] arr4 = {};
        int k4 = 2;
        int threshold4 = 4;
        System.out.println("Expected: 0");
        System.out.println("Result: " + numOfSubarrays(arr4, k4, threshold4));

        // Test Case 5: Single valid subarray
        System.out.println("Test Case 5:");
        int[] arr5 = {1,2,3,4,5};
        int k5 = 3;
        int threshold5 = 3;
        System.out.println("Expected: 3");
        System.out.println("Result: " + numOfSubarrays(arr5, k5, threshold5));
    }
}

```





