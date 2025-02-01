---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "81. Search in Rotated Sorted Array II"
date: "2025-02-01"
tags: Medium
categories:
  - "LeetCode Binary Search"
---


- There is an integer array `nums` sorted in non-decreasing order (not necessarily with **distinct** values).
- Before being passed to your function, `nums` is **rotated** at an unknown pivot index `k` (`0 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,4,4,5,6,6,7]` might be rotated at pivot index `5` and become `[4,5,6,6,7,0,1,2,4,4]`.
- Given the array `nums` **after** the rotation and an integer `target`, return `true` *if* `target` *is in* `nums`*, or* `false` *if it is not in* `nums`*.*
- You must decrease the overall operation steps as much as possible.

**Example 1**

```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```

**Example 2**

```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```

## Method 1

```tex
【O(log(n))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2025/02/01
 */
public class SearchInRotatedSortedArrayII {
    public static boolean search(int[] nums, int target) {
        // Handle edge cases: empty array or null input
        if (nums == null || nums.length == 0) {
            return false;
        }

        // Initialize pointers for binary search
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            // Calculate middle point avoiding potential integer overflow
            int mid = left + (right - left) / 2;

            // If target is found at mid, return true
            if (nums[mid] == target) {
                return true;
            }

            // Handle the case where left element equals middle element
            // This is the key difference from the original rotated sorted array problem
            if (nums[left] == nums[mid]) {
                // Skip one duplicate element and continue
                left++;
                continue;
            }

            // Check if the left half is sorted
            if (nums[left] <= nums[mid]) {
                // Check if target lies in the left sorted half
                if (nums[left] <= target && target < nums[mid]) {
                    // Target is in the left half, search there
                    right = mid - 1;
                } else {
                    // Target is in the right half
                    left = mid + 1;
                }
            }
            // Right half must be sorted
            else {
                // Check if target lies in the right sorted half
                if (nums[mid] < target && target <= nums[right]) {
                    // Target is in the right half, search there
                    left = mid + 1;
                } else {
                    // Target is in the left half
                    right = mid - 1;
                }
            }
        }

        // Target was not found in the array
        return false;
    }

    public static void main(String[] args) {

        // Here we test our solution with five common cases.
        System.out.println(search(new int[]{2,5,6,0,0,1,2}, 0)); // We expect: true
        System.out.println(search(new int[]{2,5,6,0,0,1,2}, 3)); // We expect: false
        System.out.println(search(new int[]{4,5,6,7,0,1,2}, 0)); // We expect: true
        System.out.println(search(new int[]{4,5,6,7,0,1,2}, 3)); // We expect: false
        System.out.println(search(new int[]{1,1,1,2,1}, 2));     // We expect: true
    }
}


```

