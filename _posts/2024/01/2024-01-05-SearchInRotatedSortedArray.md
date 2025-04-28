---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "33.Search in Rotated Sorted Array"
date: "2024-01-05"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 33. Search in Rotated Sorted Array [Medium]

- There is an integer array `nums` sorted in ascending order (with **distinct** values).
- Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.
- Given the array `nums` **after** the possible rotation and an integer `target`, return *the index of* `target` *if it is in* `nums`*, or* `-1` *if it is not in* `nums`.
- You must write an algorithm with `O(log n)` runtime complexity.

**Example 1**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

## Method 1

```tex
【O(log(n))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2024/01/26
 */
public class SearchInRotatedSortedArray {
    public static int search(int[] nums, int target) {
        // Define left and right pointers for binary search
        int left = 0, right = nums.length - 1;

        // Loop condition for binary search
        while (left <= right) {
            // Calculate middle index of the array
            int mid = left + (right - left) / 2;

            // If the array element at middle index is equal to target, return the index
            if (nums[mid] == target){
                return mid;
            }
            // If a sorted subarray is located on the left side
            else if (nums[mid] >= nums[left]) {
                // If the target is within this sorted subarray, move the right pointer to the left (narrow down the search scope)
                if (target >= nums[left] && target < nums[mid]){
                    right = mid - 1;
                }
                // If the target is not within this sorted subarray, move the left pointer to the right (narrow down the search scope)
                else {
                    left = mid + 1;
                }
            }
            // If a sorted subarray is located on the right side
            else {
                // If the target is within this sorted subarray, move the left pointer to the right (narrow down the search scope)
                if (target <= nums[right] && target > nums[mid]){
                    left = mid + 1;
                }
                // If the target is not within this sorted subarray, move the right pointer to the left (narrow down the search scope)
                else {
                    right = mid - 1;
                }
            }
        }

        // Return -1 if target is not found in the array
        return -1;
    }

    // Test Cases
    public static void main(String[] args) {
        System.out.println(search(new int[]{4,5,6,7,0,1,2}, 0));  // Expected Output: 4
        System.out.println(search(new int[]{4,5,6,7,0,1,2}, 3));  // Expected Output: -1
        System.out.println(search(new int[]{1}, 0));  // Expected Output: -1
    }
}

```















