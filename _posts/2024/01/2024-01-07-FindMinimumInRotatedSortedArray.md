---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "153.Find Minimum in Rotated Sorted Array"
date: "2024-01-07"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 153.Find Minimum in Rotated Sorted Array [Medium]

- Suppose an array of length `n` sorted in ascending order is rotated between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:
  - `[4,5,6,7,0,1,2]` if it was rotated `4` times.
  - `[0,1,2,4,5,6,7]` if it was rotated `7` times.
- Notice that rotating an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.
- Given the sorted rotated array `nums` of unique elements, return *the minimum element of this array*.
- You must write an algorithm that runs in `O(log n) time.`

**Example 1**

```
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

**Example 2**

```
Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
```

**Example 3**

```
Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times. 
```



## Method 1

```tex
【O(log(n))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2024/02/12
 */
public class FindMinimumInRotatedSortedArray {

    public static int findMin(int[] nums) {
        // Define the left and right pointers
        int left = 0;
        int right = nums.length - 1;

        // Binary search
        while (left < right) {
            // Calculate the middle index
            int mid = left + (right - left) / 2;

            // If the middle element is larger than the rightmost element, the minimum element should be on the right half of the array,
            // so we move the left pointer to the right
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                // Otherwise, the minimum element should be on the left half of the array, so we move the right pointer to the left
                right = mid;
            }
        }

        // When the left pointer meets the right pointer, the element they are pointing to is the minimum element
        return nums[left];
    }

    /**
     * The main method where the method findMin is tested with some examples
     */
    public static void main(String[] args) {

        int[] nums1 = {3,4,5,1,2};
        System.out.println(findMin(nums1));  // Expected output: 1

        int[] nums2 = {4,5,6,7,0,1,2};
        System.out.println(findMin(nums2));  // Expected output: 0

        int[] nums3 = {11,13,15,17};
        System.out.println(findMin(nums3));  // Expected output: 11
    }
}

```

