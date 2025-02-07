---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "220. Contains Duplicate III"
date: "2025-01-10"
tags: Hard SlideWindow
categories:
  - "LeetCode SlideWindow"
---

- You are given an integer array `nums` and two integers `indexDiff` and `valueDiff`.
- Find a pair of indices `(i, j)` such that:
  - `i != j`,
  - `abs(i - j) <= indexDiff`.
  - `abs(nums[i] - nums[j]) <= valueDiff`, and
- Return `true` *if such pair exists or* `false` *otherwise*.

**Example 1**

```
Input: nums = [1,2,3,1], indexDiff = 3, valueDiff = 0
Output: true
Explanation: We can choose (i, j) = (0, 3).
We satisfy the three conditions:
i != j --> 0 != 3
abs(i - j) <= indexDiff --> abs(0 - 3) <= 3
abs(nums[i] - nums[j]) <= valueDiff --> abs(1 - 1) <= 0
```

**Example 2**

```
Input: nums = [1,5,9,1,5,9], indexDiff = 2, valueDiff = 3
Output: false
Explanation: After trying all the possible pairs (i, j), we cannot satisfy the three conditions, so we return false.
```

## Method 1

```tex
【O(n * log(k)) time | O(n) space】
```

```java
package Leetcode.SlideWindow;

import java.util.TreeSet;

/**
 * @author zhengxingxing
 * @date 2025/01/10
 */
public class ContainsDuplicateIII {
    public static boolean containsNearbyAlmostDuplicate(int[] nums, int indexDiff, int valueDiff) {
        // Handle edge cases: null array, array length less than 2, invalid indexDiff or valueDiff
        if (nums == null || nums.length < 2 || indexDiff < 1 || valueDiff < 0) {
            return false;
        }

        TreeSet<Long> set = new TreeSet<>();

        for (int i = 0; i < nums.length; i++) {
            // 1. Enter window
            // Convert current number to long to avoid integer overflow
            Long curr = (long) nums[i];
            // Find the smallest value greater than or equal to (curr - valueDiff)
            Long ceiling = set.ceiling(curr - valueDiff);
            // If such value exists and is within the valueDiff range, we found a valid pair
            if (ceiling != null && ceiling <= curr + valueDiff) {
                return true;
            }
            set.add(curr);

            if (i < indexDiff) { // Window size has not reached indexDiff yet
                continue;
            }

            // 2. Update answer
            // No need to update answer here as we return true immediately when finding a valid pair

            // 3. Exit window
            // Remove the leftmost element from the window
            set.remove((long) nums[i - indexDiff]);
        }

        return false;
    }

    public static void main(String[] args) {
        // Test Case 1: Should return true
        // Contains duplicate values (1) within indexDiff = 3 and valueDiff = 0
        int[] nums1 = {1, 2, 3, 1};
        int indexDiff1 = 3;
        int valueDiff1 = 0;
        System.out.println("Test Case 1 Result: " + containsNearbyAlmostDuplicate(nums1, indexDiff1, valueDiff1));
        // Expected output: true

        // Test Case 2: Should return false
        // No pairs satisfy both indexDiff and valueDiff conditions
        int[] nums2 = {1, 5, 9, 1, 5, 9};
        int indexDiff2 = 2;
        int valueDiff2 = 3;
        System.out.println("Test Case 2 Result: " + containsNearbyAlmostDuplicate(nums2, indexDiff2, valueDiff2));
        // Expected output: false

        // Test Case 3: Edge case - single element array
        // Should return false as no pairs exist
        int[] nums3 = {1};
        int indexDiff3 = 1;
        int valueDiff3 = 1;
        System.out.println("Test Case 3 Result: " + containsNearbyAlmostDuplicate(nums3, indexDiff3, valueDiff3));
        // Expected output: false
    }
}
```





