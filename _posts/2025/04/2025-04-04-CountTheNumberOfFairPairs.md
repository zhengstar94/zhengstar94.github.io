---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "(Review)2563. Count the Number of Fair Pairs"
date: "2025-04-04"
tags: Medium TwoPointers Review
categories:
  - "LeetCode ThreePointers"
---

- Given a **0-indexed** integer array `nums` of size `n` and two integers `lower` and `upper`, return *the number of fair pairs*.
- A pair `(i, j)` is **fair** if:
  - `0 <= i < j < n`, and
  - `lower <= nums[i] + nums[j] <= upper`

**Example 1**

```
Input: nums = [0,1,7,4,4,5], lower = 3, upper = 6
Output: 6
Explanation: There are 6 fair pairs: (0,3), (0,4), (0,5), (1,3), (1,4), and (1,5).
```

**Example 2**

```
Input: nums = [1,7,9,2,5], lower = 11, upper = 11
Output: 1
Explanation: There is a single fair pair: (2,3).
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.TwoPointer.ThreePointer;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/04/04
 */
public class CountTheNumberOfFairPairs {
    public static long countFairPairs(int[] nums, int lower, int upper) {
        // Sort array to handle pairs in order
        Arrays.sort(nums);
        long ans = 0;

        // Initialize pointers to end of array
        // left: tracks the leftmost position where nums[j] + nums[i] >= lower
        // right: tracks the leftmost position where nums[j] + nums[i] > upper
        int left = nums.length;
        int right = nums.length;

        // For each number as the second element of the pair
        for (int i = 0; i < nums.length; i++) {
            // Move right pointer left while sum is too large
            // Find rightmost position where nums[right-1] + nums[i] <= upper
            while (right > 0 && nums[right - 1] + nums[i] > upper) {
                right--;
            }

            // Move left pointer left while sum is large enough
            // Find rightmost position where nums[left-1] + nums[i] >= lower
            while (left > 0 && nums[left - 1] + nums[i] >= lower) {
                left--;
            }

            /**
             * Calculate valid pairs for current nums[i]:
             *
             * Math.min(right, i) - Math.min(left, i) counts pairs where:
             * 1. The first number (nums[j]) must be to the left of nums[i] (j < i)
             * 2. The sum must be <= upper (controlled by right pointer)
             * 3. The sum must be >= lower (controlled by left pointer)
             *
             * Example: for i = 2 (nums[2] = 5):
             * - If right = 3 and left = 0:
             * - Math.min(3, 2) = 2 (can only use positions up to i)
             * - Math.min(0, 2) = 0 (start from position 0)
             * - 2 - 0 = 2 pairs possible (using positions 0 and 1)
             *
             * We use Math.min because:
             * - We can't use positions beyond i (need j < i)
             * - right gives upper bound of valid positions
             * - left gives lower bound of valid positions
             * - The difference gives count of valid positions
             */
            ans += Math.min(right, i) - Math.min(left, i);
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test case 1: Multiple fair pairs
        int[] nums1 = {0, 1, 7, 4, 4, 5};
        int lower1 = 3;
        int upper1 = 6;
        System.out.println("Test Case 1");
        System.out.println("Input: nums = [0,1,7,4,4,5], lower = 3, upper = 6");
        System.out.println("Expected Output: 6");
        System.out.println("Actual Output: " + countFairPairs(nums1, lower1, upper1));
        System.out.println();

        // Test case 2: Single fair pair
        int[] nums2 = {1, 7, 9, 2, 5};
        int lower2 = 11;
        int upper2 = 11;
        System.out.println("Test Case 2");
        System.out.println("Input: nums = [1,7,9,2,5], lower = 11, upper = 11");
        System.out.println("Expected Output: 1");
        System.out.println("Actual Output: " + countFairPairs(nums2, lower2, upper2));
    }
}

```





