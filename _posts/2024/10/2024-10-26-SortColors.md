---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "75.Sort Colors"
date: "2024-10-26"
categories:
  - "LeetCode TwoPointers"
---

- Given an array `nums` with `n` objects colored red, white, or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.
- We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.
- You must solve this problem without using the library's sort function.

**Example 1**

```
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

**Example 2**

```
Input: nums = [2,0,1]
Output: [0,1,2]
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengstars
 * @date 2024/10/26
 */
public class SortColors {
    public static void sortColors(int[] nums) {
        int low = 0, mid = 0, high = nums.length - 1;

        // Iterate until mid passes high
        while (mid <= high) {
            if (nums[mid] == 0) {
                // Case 1: Current element is 0, swap with the element at 'low' pointer
                int temp = nums[low];
                nums[low] = nums[mid];
                nums[mid] = temp;
                // Increment both low and mid pointers after swap
                mid++;
                low++;
            } else if (nums[mid] == 1) {
                // Case 2: Current element is 1, it's already in the correct place
                // Just move the mid pointer to the right
                mid++;
            } else {
                // Case 3: Current element is 2, swap with the element at 'high' pointer
                int temp = nums[mid];
                nums[mid] = nums[high];
                nums[high] = temp;
                // Decrement high pointer, but keep mid at the same position to recheck swapped element
                high--;
            }
        }
    }

    public static void main(String[] args) {
        int[] nums1 = {2, 0, 2, 1, 1, 0};
        int[] nums2 = {2, 0, 1};

        System.out.println("Before sortColors:");
        printArray(nums1);
        sortColors(nums1);
        System.out.println("After sortColors:");
        printArray(nums1);

        System.out.println("\nBefore sortColors:");
        printArray(nums2);
        sortColors(nums2);
        System.out.println("After sortColors:");
        printArray(nums2);
    }

    private static void printArray(int[] nums) {
        for (int num : nums) {
            System.out.print(num + " ");
        }
        System.out.println();
    }
}

```
