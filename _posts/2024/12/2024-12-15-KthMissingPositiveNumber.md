---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1539. Kth Missing Positive Number"
date: "2024-12-15"
tags: Easy
categories:
  - "LeetCode BinarySearch"
---


- Given an array `arr` of positive integers sorted in a **strictly increasing order**, and an integer `k`.
- Return *the* `kth` ***positive** integer that is **missing** from this array.*

**Example 1**

```
Input: arr = [2,3,4,7,11], k = 5
Output: 9
Explanation: The missing positive integers are [1,5,6,8,9,10,12,13,...]. The 5th missing positive integer is 9.
```

**Example 2**

```
Input: arr = [1,2,3,4], k = 2
Output: 6
Explanation: The missing positive integers are [5,6,7,...]. The 2nd missing positive integer is 6.
```

## Method 1

```tex
【O(log(n) time | O(1) space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengxingxing
 * @date 2024/12/15
 */
public class KthMissingPositiveNumber {
    public static int findKthPositive(int[] arr, int k) {
        int left = 0, right = arr.length - 1;

        // Binary search to find the first position where the number of missing numbers >= k
        while (left <= right) {
            int mid = left + (right - left) / 2;
            // Calculate the number of missing numbers up to arr[mid]
            int missingCount = arr[mid] - (mid + 1);
            if (missingCount < k) {
                // If the missing count is less than k, search to the right half
                left = mid + 1;
            } else {
                // If the missing count is greater or equal to k, search to the left half
                right = mid - 1;
            }
        }

        // When the loop ends, left is the position where the first missing count >= k is found
        return left + k;
    }

    public static void main(String[] args) {
        System.out.println(findKthPositive(new int[]{2,3,4,7,11}, 5));
    }
}

```





