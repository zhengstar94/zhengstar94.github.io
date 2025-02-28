---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1471. The k Strongest Values in an Array"
date: "2025-02-21"
tags: Medium TwoPointers
categories:
  - "LeetCode SingleSeqTwoPointersInward"
---


- Given an array of integers `arr` and an integer `k`.
- A value `arr[i]` is said to be stronger than a value `arr[j]` if `|arr[i] - m| > |arr[j] - m|` where `m` is the **median** of the array. If `|arr[i] - m| == |arr[j] - m|`, then `arr[i]` is said to be stronger than `arr[j]` if `arr[i] > arr[j]`.
- Return *a list of the strongest `k`* values in the array. return the answer **in any arbitrary order**.
- **Median** is the middle value in an ordered integer list. More formally, if the length of the list is n, the median is the element in position `((n - 1) / 2)` in the sorted list **(0-indexed)**.
  - For `arr = [6, -3, 7, 2, 11]`, `n = 5` and the median is obtained by sorting the array `arr = [-3, 2, 6, 7, 11]` and the median is `arr[m]` where `m = ((5 - 1) / 2) = 2`. The median is `6`.
  - For `arr = [-7, 22, 17, 3]`, `n = 4` and the median is obtained by sorting the array `arr = [-7, 3, 17, 22]` and the median is `arr[m]` where `m = ((4 - 1) / 2) = 1`. The median is `3`.

**Example 1**

```
Input: arr = [1,2,3,4,5], k = 2
Output: [5,1]
Explanation: Median is 3, the elements of the array sorted by the strongest are [5,1,4,2,3]. The strongest 2 elements are [5, 1]. [1, 5] is also accepted answer.
Please note that although |5 - 3| == |1 - 3| but 5 is stronger than 1 because 5 > 1.
```

**Example 2**

```
Input: arr = [1,1,3,5,5], k = 2
Output: [5,5]
Explanation: Median is 3, the elements of the array sorted by the strongest are [5,5,1,1,3]. The strongest 2 elements are [5, 5].
```

**Example 3**

```
Input: arr = [6,7,11,7,6,8], k = 5
Output: [11,8,6,6,7]
Explanation: Median is 7, the elements of the array sorted by the strongest are [11,8,6,6,7,7].
Any permutation of [11,8,6,6,7] is accepted.
```

## Method 1

```tex
【O(nlog(n)) time | O(k) space】
```

```java
package Leetcode.TwoPointer;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/02/21
 */
public class TheKStrongestValuesInAnArray {
    
    public static int[] getStrongest(int[] arr, int k) {
        // Sort the array to find median
        Arrays.sort(arr);
        int n = arr.length;

        // Calculate median: for array of length n, median is at position (n-1)/2
        int median = arr[(n - 1) / 2];

        // Initialize result array to store k strongest elements
        int[] result = new int[k];

        // Initialize two pointers: left points to start, right points to end
        int left = 0;
        int right = n - 1;
        int index = 0;

        // Continue until we find k strongest elements
        while (index < k) {
            // Calculate absolute difference from median for both pointers
            int leftDiff = Math.abs(arr[left] - median);
            int rightDiff = Math.abs(arr[right] - median);

            // If left element has greater difference, it's stronger
            if (leftDiff > rightDiff) {
                result[index] = arr[left];
                left++;
            }
            // If right element has greater difference, it's stronger
            else if (leftDiff < rightDiff) {
                result[index] = arr[right];
                right--;
            }
            // If differences are equal, compare actual values
            else {
                // When differences are equal, larger value is stronger
                if (arr[left] > arr[right]) {
                    result[index] = arr[left];
                    left++;
                } else {
                    result[index] = arr[right];
                    right--;
                }
            }
            index++;
        }

        return result;
    }

    
    public static void main(String[] args) {
        // Test Case 1: Regular case with distinct elements
        int[] arr1 = {1, 2, 3, 4, 5};
        int k1 = 2;
        System.out.println("Test Case 1 Result: " + Arrays.toString(getStrongest(arr1, k1)));

        // Test Case 2: Array with duplicate elements
        int[] arr2 = {1, 1, 3, 5, 5};
        int k2 = 2;
        System.out.println("Test Case 2 Result: " + Arrays.toString(getStrongest(arr2, k2)));

        // Test Case 3: Larger array with more elements
        int[] arr3 = {6, 7, 11, 7, 6, 8};
        int k3 = 5;
        System.out.println("Test Case 3 Result: " + Arrays.toString(getStrongest(arr3, k3)));
    }
}

```





