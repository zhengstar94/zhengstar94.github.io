---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "162.Find Peak Element"
date: "2024-01-09"
categories:
  - "LeetCode Binary Search"
---

# LeetCode 162. Find Peak Element [Medium]

- A peak element is an element that is greater than its neighbors.
- Given an input array `nums`, where `nums[i] ≠ nums[i+1]`, find a peak element and return its index.
- The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.
- You may imagine that `nums[-1] = nums[n] = -∞`.

**Example 1**

```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

**Example 2**

```
Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5 
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
```

## Method 1

```tex
【O(log(n))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2024/02/13
 */
public class FindPeakElement {
    public static int findPeakElement(int[] nums) {
        // set the left boundary of the search range as the first number of the array
        int left = 0;
        // set the right boundary of the search range as the last number of the array
        int right = nums.length - 1;

        // keep the searching until the search range is more than one
        while (left < right) {
            // find the middle number of current search range
            int mid = left + (right - left) / 2;

            // if the middle number is less than the next number, which means the peak must be in the right half
            // so we move the left boundary to the middle point plus 1
            if (nums[mid] < nums[mid + 1]) {
                left = mid + 1;
            }
            // if the middle number is not less than the next number, which means the peak must be in the left half including current middle number
            // so we move the right boundary to the middle point
            else {
                right = mid;
            }
        }

        // return the remaining candidate as the result
        return left;
    }
    
    public static void main(String[] args) {

        // test the findPeakElement method on a few test cases
        // Print out the answer and compare it with expected output to see whether the solution is right.
        int[] nums1 = {1, 2, 3, 1};
        System.out.println(findPeakElement(nums1));  // Output: 2

        int[] nums2 = {1, 2, 1, 3, 5, 6, 4};
        System.out.println(findPeakElement(nums2));  // Output: 1 or 5
    }
}

```

