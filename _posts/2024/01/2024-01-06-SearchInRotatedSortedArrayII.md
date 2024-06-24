---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "81.Search in Rotated Sorted Array II"
date: "2024-01-06"
categories:
  - "LeetCode Binary Search"
---

# LeetCode 81. Search in Rotated Sorted Array II [Medium]

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
 * @date 2024/02/10
 */
public class SearchInRotatedSortedArrayII {
    public static boolean search(int[] nums, int target) {
        // Check for an empty sorted array
        if (nums == null || nums.length == 0) {
            return false;
        }

        // Initializing the leftmost and rightmost pointers
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            // Compute the middle index
            int mid = left + (right - left) / 2;

            // If target is found, return true
            if (nums[mid]==target) {
                return true;
            }

            // Check if the first half of the array is sorted
            if (nums[left] <= nums[mid]) {

                // If the target lies in the sorted range, adjust the right pointer to mid - 1
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    // If target does not lie in the sorted range, adjust the left pointer to mid + 1
                    left = mid + 1;
                }
            } else {
                // If first half is not sorted, the second half must be sorted

                // If target lies in the second sorted half,
                // adjust the left pointer to mid + 1
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    // If target does not lie in the second sorted half,
                    // adjust the right pointer to mid - 1
                    right = mid - 1;
                }
            }
        }

        // If the target is not found in the entire searching process, return false
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

