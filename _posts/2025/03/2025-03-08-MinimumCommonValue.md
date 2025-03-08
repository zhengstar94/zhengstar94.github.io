---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2540. Minimum Common Value"
date: "2025-03-08"
tags: Easy TwoPointers
categories:
  - "LeetCode DoubleSeqTwoPointers" 
---

- Given two integer arrays `nums1` and `nums2`, sorted in non-decreasing order, return *the **minimum integer common** to both arrays*. If there is no common integer amongst `nums1` and `nums2`, return `-1`.
- Note that an integer is said to be **common** to `nums1` and `nums2` if both arrays have **at least one** occurrence of that integer.

**Example 1**

```
Input: nums1 = [1,2,3], nums2 = [2,4]
Output: 2
Explanation: The smallest element common to both arrays is 2, so we return 2.
```

**Example 2**

```
Input: nums1 = [1,2,3,6], nums2 = [2,3,4,5]
Output: 2
Explanation: There are two common elements in the array 2 and 3 out of which 2 is the smallest, so 2 is returned.
```

## Method 1

```tex
【O(min(n, m)) time | O(1) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqTwoPointers;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/03/08
 */
public class MinimumCommonValue {

    public static int getCommon(int[] nums1, int[] nums2) {
        // Initialize two pointers, each pointing to the start of respective arrays
        int i = 0, j = 0;

        // Continue until we reach the end of either array
        while (i < nums1.length && j < nums2.length) {
            // If we find matching elements, we've found the minimum common value
            if (nums1[i] == nums2[j]) {
                return nums1[i];
            }
            // If element in nums1 is smaller, move nums1's pointer forward
            else if (nums1[i] < nums2[j]) {
                i++;
            }
            // If element in nums2 is smaller, move nums2's pointer forward
            else {
                j++;
            }
        }
        // If no common element is found, return -1
        return -1;
    }

    public static void main(String[] args) {
        // Test Case 1: Basic case with common element
        int[] nums1 = {1, 2, 3};
        int[] nums2 = {2, 4};
        System.out.println("Test Case 1:");
        System.out.println("nums1: " + Arrays.toString(nums1));
        System.out.println("nums2: " + Arrays.toString(nums2));
        System.out.println("Minimum common element: " + getCommon(nums1, nums2));

        // Test Case 2: Multiple common elements
        int[] nums3 = {1, 2, 3, 6};
        int[] nums4 = {2, 3, 4, 5};
        System.out.println("\nTest Case 2:");
        System.out.println("nums1: " + Arrays.toString(nums3));
        System.out.println("nums2: " + Arrays.toString(nums4));
        System.out.println("Minimum common element: " + getCommon(nums3, nums4));

        // Test Case 3: No common elements
        int[] nums5 = {1, 2, 3};
        int[] nums6 = {4, 5, 6};
        System.out.println("\nTest Case 3:");
        System.out.println("nums1: " + Arrays.toString(nums5));
        System.out.println("nums2: " + Arrays.toString(nums6));
        System.out.println("Minimum common element: " + getCommon(nums5, nums6));
    }
}

```





