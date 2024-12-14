---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2762. Continuous Subarrays"
date: "2024-12-14"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---

- You are given a **0-indexed** integer array `nums`. A subarray of `nums` is called **continuous** if:

  - Let `i`, `i + 1`, ..., `j` be the indices in the subarray. Then, for each pair of indices `i <= i1, i2 <= j`, `0 <= |nums[i1] - nums[i2]| <= 2`.

- Return *the total number of **continuous** subarrays.*

  A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1**

```
Input: nums = [5,4,2,4]
Output: 8
Explanation: 
Continuous subarray of size 1: [5], [4], [2], [4].
Continuous subarray of size 2: [5,4], [4,2], [2,4].
Continuous subarray of size 3: [4,2,4].
Thereare no subarrys of size 4.
Total continuous subarrays = 4 + 3 + 1 = 8.
It can be shown that there are no more continuous subarrays.
```

**Example 2**

```
Input: nums = [1,2,3]
Output: 6
Explanation: 
Continuous subarray of size 1: [1], [2], [3].
Continuous subarray of size 2: [1,2], [2,3].
Continuous subarray of size 3: [1,2,3].
Total continuous subarrays = 3 + 2 + 1 = 6.
```

## Method 1

```tex
【O(n) time | O(k) space】
```

```java
package Leetcode.SlideWindow;

import java.util.TreeMap;

/**
 * @author zhengxingxing
 * @date 2024/12/14
 */
public class ContinuousSubarrays {
    public static long continuousSubarrays(int[] nums) {
        // Left boundary pointer, initially set to 0
        int left = 0;

        // Variable to store the total count of continuous subarrays
        long ans = 0;

        // Use a TreeMap to maintain elements in the window and their frequencies
        // TreeMap allows quick access to the maximum and minimum values
        TreeMap<Integer, Integer> t = new TreeMap<>();

        // Iterate through the array using the right boundary pointer
        for (int right = 0; right < nums.length; right++) {
            // Add the current element to the window, increasing its frequency
            // If the element is not present, initialize its frequency to 1; otherwise, increment it
            t.put(nums[right], t.getOrDefault(nums[right], 0) + 1);

            // Shrink the window when the difference between the maximum and minimum values exceeds 2
            while (t.lastKey() - t.firstKey() > 2) {
                // Get the element at the left boundary
                int key = nums[left++];

                // If the element appears only once, remove it from the TreeMap
                // Otherwise, decrease its frequency
                if (t.get(key) == 1) {
                    t.remove(key);
                } else {
                    t.put(key, t.get(key) - 1);
                }
            }

            // Calculate the number of continuous subarrays ending at the current right boundary
            // right - left + 1 represents all possible subarrays in the current window
            ans += right - left + 1;
        }

        // Return the total count of continuous subarrays
        return ans;
    }

    public static void main(String[] args) {

        // Test case 1: Example array
        int[] nums1 = {5, 4, 2, 4};
        System.out.println("Test Case 1: " + continuousSubarrays(nums1));
        // Expected output: 8

        // Test case 2: Increasing sequence
        int[] nums2 = {1, 2, 3};
        System.out.println("Test Case 2: " + continuousSubarrays(nums2));
        // Expected output: 6

        // Test case 3: All elements are the same
        int[] nums3 = {1, 1, 1, 1};
        System.out.println("Test Case 3: " + continuousSubarrays(nums3));
        // Expected output: 10

        // Test case 4: Array with large differences
        int[] nums4 = {1, 5, 6, 7, 8};
        System.out.println("Test Case 4: " + continuousSubarrays(nums4));
        // Expected output: 10
    }
}

```





