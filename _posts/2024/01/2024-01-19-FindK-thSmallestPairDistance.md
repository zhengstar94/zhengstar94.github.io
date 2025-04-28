---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "719. Find K-th Smallest Pair Distance"
date: "2024-01-19"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 719. Find K-th Smallest Pair Distance [Hard]

- Given an integer array, return the k-th smallest **distance** among all the pairs. The distance of a pair (A, B) is defined as the absolute difference between A and B.

**Example 1**

```
Input:
nums = [1,3,1]
k = 1
Output: 0 
Explanation:
Here are all the pairs:
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
Then the 1st smallest distance pair is (1,1), and its distance is 0.
```

## Method 1

```tex
【O(nlog(m))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/02/04
 */
public class FindKthSmallestPairDistance {
    public static int smallestDistancePair(int[] nums, int k) {
        // Firstly, sort the array
        Arrays.sort(nums);

        // Set the left and right boundary for binary search
        int left = 0;
        int right = nums[nums.length - 1] - nums[0];

        while (left <= right) {
            // Calculate the mid-value
            int mid = left + (right - left) / 2;

            // Counter to record the quantity of number pairs with current distance
            int count = 0;
            int j = 0;
            // Traverse the array, use sliding window approach to get the quantity of number pairs with a distance not bigger than mid
            for (int i = 0; i < nums.length; ++i) {
                // Traverse the array to find out the j that satisfy nums[j] - nums[i] <= mid
                while (j < nums.length && nums[j] - nums[i] <= mid) {
                    ++j;
                }
                // Update count, j - i - 1 is the quantity of j that meet the condition
                count += j - i - 1;
            }

            // Adjust the boundary of binary search according to the quantity of number pairs
            if (count >= k) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }

        // The left boundary is the k-th smallest distance
        return left;
    }

    public static void main(String[] args) {

        // Define test cases
        int[] nums = new int[]{1, 1, 3, 5, 8};
        int k = 7;

        // Call the method and print the result
        int result = smallestDistancePair(nums, k);
        System.out.println(result);
    }
}
```

