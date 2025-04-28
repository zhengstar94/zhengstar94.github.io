---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "154.Find Minimum in Rotated Sorted Array II"
date: "2024-01-08"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 154. Find Minimum in Rotated Sorted Array II [Medium]

- Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

  (i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

- Find the minimum element.

- The array may contain duplicates.

**Example 1**

```
Input:
 [1,3,5]

Output:
 1
```

**Example 2**

```
Input:
 [2,2,2,0,1]

Output:
 0
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
public class FindMinimumInRotatedSortedArrayII {
    public static int findMin(int[] nums) {
        // set the left boundary of the search range as the first number of the array
        int left = 0;
        // set the right boundary of the search range as the last number of the array
        int right = nums.length - 1;

        // keep the searching until the search range is more than one
        while (left < right) {
            // find the middle number of current search range
            int mid = left + (right - left) / 2;

            // if the middle number is greater than the right boundary number,
            // that means the minimum number locates in the right half
            // so we move the left boundary to the middle point plus 1
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            }
            // if the middle number is less than the right boundary number,
            // that means the minimum number locates in the left half including current middle number
            // so we move the right boundary to the middle point
            else if (nums[mid] < nums[right]) {
                right = mid;
            }
            // if the middle number equals to the right boundary number,
            // we cannot determine the minimum number is in the left or right half
            // so we can only shrink the right boundary by 1 to continue
            else {
                right = right - 1;
            }
        }

        // return the remaining candidate as the result
        return nums[left];
    }

    /**
     * The main method where the method findMin is tested with some examples
     */
    public static void main(String[] args) {

        // test the findMin method on a few test cases
        // Print out the answer and compare it with expected output to see whether the solution is right.

        int[] nums1 = {1,3,5};
        System.out.println(findMin(nums1));  // Expected output: 1

        int[] nums2 = {2,2,2,0,1};
        System.out.println(findMin(nums2));  // Expected output: 0

        int[] nums3 = {3,3,1,3};
        System.out.println(findMin(nums3));  // Expected output: 1
    }
}

```

