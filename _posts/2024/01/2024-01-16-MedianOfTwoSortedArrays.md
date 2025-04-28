---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "4. Median of Two Sorted Arrays"
date: "2024-01-16"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 4. Median of Two Sorted Arrays [Hard]

- There are two sorted arrays **nums1** and **nums2** of size m and n respectively.
- Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

**Example 1**

```
nums1 = [1, 3]
nums2 = [2]
 
The median is 2.0
```

**Example 2**

```
nums1 = [1, 2]
nums2 = [3, 4]
 
The median is (2 + 3)/2 = 2.5
```

## Method 1

```tex
【O(log(m+n))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2024/01/30
 */
public class MedianOfTwoSortedArrays {
    public static double findMedianSortedArrays(int[] nums1, int[] nums2) {
        /* Make sure we always do binary search for the shorter array.
           If nums1's length is more than nums2's, we swap them.
        */
        if (nums1.length > nums2.length) {
            return findMedianSortedArrays(nums2, nums1);
        }

        // Obtain the lengths of two arrays
        int n1 = nums1.length;
        int n2 = nums2.length;
        /* 'k' is the position of virtual combined array (of nums1 and nums2)
           So, the median is (kth smallest element + (k+1)th smallest element)/2 if elements are even,
           or kth smallest element if elements are odd.
        */
        int k = (n1 + n2 + 1) / 2;

        // Left and Right pointers for binary search in nums1
        int l = 0;
        int r = n1;

        // Binary search in nums1
        while (l < r) {
            int m1 = l + (r - l) / 2;
            int m2 = k - m1;
            /* If nums1[m1] < nums2[m2-1], then the median number is definitely not in nums1[l, m1], so we increase l to m1 + 1.
               If nums1[m1] >= nums2[m2-1], the median number is definitely not in nums2[m2, n2], so we decrease r to m1.
            */
            if (nums1[m1] < nums2[m2 - 1]) {
                l = m1 + 1;
            } else {
                r = m1;
            }
        }

        /* m1 are the actual numbers of elements in array nums1 and nums2 smaller than the median.
           Get max of the two elements maximum of nums1 and nums2
           If total elements (from nums1 and nums2 combined) are odd, return c1.
        */
        int m1 = l;
        int m2 = k - l;

        int c1 = Math.max(m1 <= 0 ? Integer.MIN_VALUE : nums1[m1 - 1],
                m2 <= 0 ? Integer.MIN_VALUE : nums2[m2 - 1]);
        if ((n1 + n2) % 2 == 1) {
            return c1;
        }

        /* If total elements (from nums1 and nums2 combined) are even, need to return average of c1 and c2,
           where, c2 is minimum of next element of c1 in array nums1 and nums2.
        */
        int c2 = Math.min(m1 >= n1 ? Integer.MAX_VALUE : nums1[m1], m2 >= n2 ? Integer.MAX_VALUE : nums2[m2]);

        // Median is average of c1 and c2 for even case
        return (c1 + c2) * 0.5;
    }

    public static void main(String[] args) {
        int[] nums1 = {1, 3};
        int[] nums2 = {2};
        double median = findMedianSortedArrays(nums1, nums2);
        System.out.println(median);  // 2.0

        int[] nums3 = {1, 2};
        int[] nums4 = {3, 4};
        double median2 = findMedianSortedArrays(nums3, nums4);
        System.out.println(median2);  // 2.5
    }
}

```

