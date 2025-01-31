---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2831. Find the Longest Equal Subarray"
date: "2025-01-23"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindowMax"
---


- You are given a **0-indexed** integer array `nums` and an integer `k`.
- A subarray is called **equal** if all of its elements are equal. Note that the empty subarray is an **equal** subarray.
- Return *the length of the **longest** possible equal subarray after deleting **at most*** `k` *elements from* `nums`.
- A **subarray** is a contiguous, possibly empty sequence of elements within an array.

**Example 1**

```
Input: nums = [1,3,2,3,1,3], k = 3
Output: 3
Explanation: It's optimal to delete the elements at index 2 and index 4.
After deleting them, nums becomes equal to [1, 3, 3, 3].
The longest equal subarray starts at i = 1 and ends at j = 3 with length equal to 3.
It can be proven that no longer equal subarrays can be created.
```

**Example 2**

```
Input: nums = [1,1,2,2,1,1], k = 2
Output: 4
Explanation: It's optimal to delete the elements at index 2 and index 3.
After deleting them, nums becomes equal to [1, 1, 1, 1].
The array itself is an equal subarray, so the answer is 4.
It can be proven that no longer equal subarrays can be created.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/01/23
 */
public class FindTheLongestEqualSubarray {
    public static int longestEqualSubarray(List<Integer> nums, int k) {
        // Get the length of input array
        int n = nums.size();
        // Create an array of ArrayLists to store position differences for each number
        // Index represents the number, and the list stores its position differences
        List<Integer>[] posLists = new ArrayList[n + 1];

        // Initialize each ArrayList in posLists
        Arrays.setAll(posLists, i -> new ArrayList<>());

        // First loop: Build the position difference lists
        // For each number, calculate and store its position difference
        // Position difference = current index - number of times this number has appeared
        for (int i = 0; i < n; i++) {
            int x = nums.get(i);
            posLists[x].add(i - posLists[x].size());
        }

        // Initialize the answer variable to store the maximum length
        int ans = 0;

        // Second loop: Process each number's position difference list
        for (List<Integer> pos : posLists) {
            // Optimization: Skip if current list size is not greater than current answer
            // Because it cannot produce a longer subsequence
            if (pos.size() <= ans) {
                continue;
            }

            // Initialize left pointer for sliding window
            int left = 0;

            // Third loop: Slide the window to find the longest valid subsequence
            for (int right = 0; right < pos.size(); right++) {
                // While loop: Adjust the window size when too many elements need to be deleted
                // If difference between positions is greater than k,
                // it means we need to delete more elements than allowed (k)
                while (pos.get(right) - pos.get(left) > k) {
                    // Move left pointer to shrink the window
                    left++;
                }
                // Update answer with maximum window size found so far
                // right - left + 1 represents the current window size
                ans = Math.max(ans, right - left + 1);
            }
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test case 1: Expected output is 3
        List<Integer> nums1 = Arrays.asList(1,3,2,3,1,3);
        System.out.println("Test case 1 result: " + longestEqualSubarray(nums1, 3));

        // Test case 2: Expected output is 4
        List<Integer> nums2 = Arrays.asList(1,1,2,2,1,1);
        System.out.println("Test case 2 result: " + longestEqualSubarray(nums2, 2));

        // Test case 3: Expected output is 4
        List<Integer> nums3 = Arrays.asList(1, 3, 3, 4, 3, 5, 3);
        System.out.println("Test case 3 result: " + longestEqualSubarray(nums3, 2));
    }
}

```





