---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "719. Find K-th Smallest Pair Distance"
date: "2025-05-21"
tags: Hard BinarySearch
categories:
  - "LeetCode BinarySearch.KthElement"
---

- The **distance of a pair** of integers `a` and `b` is defined as the absolute difference between `a` and `b`.
- Given an integer array `nums` and an integer `k`, return *the* `kth` *smallest **distance among all the pairs*** `nums[i]` *and* `nums[j]` *where* `0 <= i < j < nums.length`.

**Example 1**

```
Input: nums = [1,3,1], k = 1
Output: 0
Explanation: Here are all the pairs:
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
Then the 1st smallest distance pair is (1,1), and its distance is 0.
```

**Example 2**

```
Input: nums = [1,1,1], k = 2
Output: 0
```

**Example 3**

```
Input: nums = [1,6,1], k = 3
Output: 5
```

## Method 1

```tex
【O(n×(logn+logD)) time | O(log(n)) space】
```

```java
package Leetcode.BinarySearch.KthElement;

import java.util.Arrays;

/**
 * @Author zhengxingxing
 * @Date 2025/05/21
 */
public class FindKthSmallestPairDistance {

    public int smallestDistancePair(int[] nums, int k) {
        // Sort the input array to allow efficient counting of pairs with two pointers
        Arrays.sort(nums);
        int n = nums.length;

        // The smallest possible distance is 0 (duplicates), largest possible is max - min
        int left = 0;
        int right = nums[n - 1] - nums[0];

        // Binary search over the distance range
        while (left <= right){
            int mid = left + (right - left) / 2;
            int cnt = 0;  // count of pairs with distance <= mid

            // Use two pointers to count pairs with distance <= mid efficiently
            for (int i = 0, j = 0; j < n; j++) {
                // For each position j in the array, we want to find how many elements nums[i]
                // (with i < j) satisfy the condition: nums[j] - nums[i] <= mid
                // Here mid is our current guess for the maximum allowed pair distance.

                // Move the left pointer i forward until the distance between nums[j] and nums[i]
                // is not greater than mid.
                // This means if the difference is too big (nums[j] - nums[i] > mid),
                // we need to increase i to reduce the distance.
                while (nums[j] - nums[i] > mid) {
                    i++;  // Shift i rightward to find smaller difference
                }

                // Now, all elements from nums[i] up to nums[j-1] form pairs with nums[j] whose distances
                // are less than or equal to mid.
                // Since the array is sorted, nums[i], nums[i+1], ..., nums[j-1] all satisfy
                // nums[j] - nums[x] <= mid (for x from i to j-1).
                //
                // The number of such pairs with nums[j] as one element is (j - i).
                // Add this count to the total count of pairs with distance <= mid.
                cnt += j - i;
            }


            // If count of pairs with distance <= mid is >= k, try to find smaller or equal distance
            if (cnt >= k){
                right = mid - 1;
            } else {
                // Otherwise, increase distance range to find larger distances
                left = mid + 1;
            }
        }

        // When the loop finishes, left is the smallest distance with at least k pairs
        return left;
    }

    public static void main(String[] args) {
        FindKthSmallestPairDistance solver = new FindKthSmallestPairDistance();

        int[] nums1 = {1, 3, 1};
        int k1 = 1;
        System.out.println("Test case 1:");
        System.out.println("Input: nums = [1,3,1], k = 1");
        System.out.println("Output: " + solver.smallestDistancePair(nums1, k1));
        System.out.println("Expected: 0\n");

        int[] nums2 = {1, 1, 1};
        int k2 = 2;
        System.out.println("Test case 2:");
        System.out.println("Input: nums = [1,1,1], k = 2");
        System.out.println("Output: " + solver.smallestDistancePair(nums2, k2));
        System.out.println("Expected: 0\n");

        int[] nums3 = {1, 6, 1};
        int k3 = 3;
        System.out.println("Test case 3:");
        System.out.println("Input: nums = [1,6,1], k = 3");
        System.out.println("Output: " + solver.smallestDistancePair(nums3, k3));
        System.out.println("Expected: 5\n");
    }
}

```





