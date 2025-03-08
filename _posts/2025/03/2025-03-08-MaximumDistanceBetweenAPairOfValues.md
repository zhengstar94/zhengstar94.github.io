---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1855. Maximum Distance Between a Pair of Values"
date: "2025-03-08"
tags: Easy TwoPointers
categories:
  - "LeetCode DoubleSeqTwoPointers" 
---

- You are given two **non-increasing 0-indexed** integer arrays `nums1` and `nums2`.
- A pair of indices `(i, j)`, where `0 <= i < nums1.length` and `0 <= j < nums2.length`, is **valid** if both `i <= j` and `nums1[i] <= nums2[j]`. The **distance** of the pair is `j - i`.
- Return *the **maximum distance** of any **valid** pair* `(i, j)`*. If there are no valid pairs, return* `0`.
- An array `arr` is **non-increasing** if `arr[i-1] >= arr[i]` for every `1 <= i < arr.length`.

**Example 1**

```
Input: nums1 = [55,30,5,4,2], nums2 = [100,20,10,10,5]
Output: 2
Explanation: The valid pairs are (0,0), (2,2), (2,3), (2,4), (3,3), (3,4), and (4,4).
The maximum distance is 2 with pair (2,4).
```

**Example 2**

```
Input: nums1 = [2,2,2], nums2 = [10,10,1]
Output: 1
Explanation: The valid pairs are (0,0), (0,1), and (1,1).
The maximum distance is 1 with pair (0,1).
```

**Example 3**

```
Input: nums1 = [30,29,19,5], nums2 = [25,25,25,25,25]
Output: 2
Explanation: The valid pairs are (2,2), (2,3), (2,4), (3,3), and (3,4).
The maximum distance is 2 with pair (2,4).
```

## Method 1

```tex
【O(n + m) time | O(1) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqTwoPointers;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/03/08
 */
public class MaximumDistanceBetweenAPairOfValues {
    
    public static int maxDistance(int[] nums1, int[] nums2) {
        // Initialize the maximum distance and two pointers
        int maxDist = 0;
        int i = 0;  // Pointer for nums1
        int j = 0;  // Pointer for nums2

        // Continue until either array is fully traversed
        while (i < nums1.length && j < nums2.length) {
            // Ensure i <= j condition is maintained
            // If i > j, advance j to catch up with i
            if (i > j) {
                j++;
                continue;
            }

            // Check if current pair is valid (nums1[i] <= nums2[j])
            if (nums1[i] <= nums2[j]) {
                // Update maximum distance if current distance is larger
                maxDist = Math.max(maxDist, j - i);
                // Move j pointer to try finding a larger distance
                j++;
            } else {
                // If current pair is invalid, move i pointer
                // Since arrays are non-increasing, moving i might find a smaller value
                i++;
            }
        }

        return maxDist;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Standard case with multiple valid pairs
        int[] nums1 = {55,30,5,4,2};
        int[] nums2 = {100,20,10,10,5};
        System.out.println("Test Case 1:");
        System.out.println("nums1: " + Arrays.toString(nums1));
        System.out.println("nums2: " + Arrays.toString(nums2));
        System.out.println("Maximum distance: " + maxDistance(nums1, nums2));

        // Test Case 2: Short arrays with multiple valid pairs
        int[] nums3 = {2,2,2};
        int[] nums4 = {10,10,1};
        System.out.println("\nTest Case 2:");
        System.out.println("nums1: " + Arrays.toString(nums3));
        System.out.println("nums2: " + Arrays.toString(nums4));
        System.out.println("Maximum distance: " + maxDistance(nums3, nums4));

        // Test Case 3: Arrays with limited valid pairs
        int[] nums5 = {30,29,19,5};
        int[] nums6 = {25,25,25,25,25};
        System.out.println("\nTest Case 3:");
        System.out.println("nums1: " + Arrays.toString(nums5));
        System.out.println("nums2: " + Arrays.toString(nums6));
        System.out.println("Maximum distance: " + maxDistance(nums5, nums6));
    }
}

```





