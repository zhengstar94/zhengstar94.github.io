---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "350. Intersection of Two Arrays II"
date: "2025-01-30"
categories:
  - "LeetCode HashTable"
---

- Given two integer arrays `nums1` and `nums2`, return *an array of their intersection*. Each element in the result must appear as many times as it shows in both arrays and you may return the result in **any order**.


**Example 1**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
```

**Example 2**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
Explanation: [9,4] is also accepted.
```

## Method 1

```tex
【O(n + m) time | O(min(n, m)) space】
```

```java
package Leetcode.HashTable;

import java.util.*;

/**
 * @author zhengxingxing
 * @date 2025/01/30
 */
public class IntersectionOfTwoArraysII {
    public static int[] intersect(int[] nums1, int[] nums2) {
        // If nums1 is longer than nums2, swap them to ensure nums1 is the shorter array
        // This optimization helps reduce space complexity by using the shorter array to create HashMap
        // For example:
        // nums1 = [1,2], length = 2
        // nums2 = [1,2,3,4,5,...,1000000], length = 1000000
        // After swap, HashMap only needs to store 2 elements instead of 1000000
        if (nums1.length > nums2.length){
            return intersect(nums2, nums1);
        }

        // Use HashMap to store the frequency of each number in nums1
        Map<Integer, Integer> map = new HashMap<>();
        // First loop: Iterate through nums1 and count frequency of each number
        // Time complexity: O(n) where n is length of nums1
        for (int num : nums1) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }

        // Store intersection results in a dynamic ArrayList
        List<Integer> intersection = new ArrayList<>();
        // Second loop: Iterate through nums2 to find intersections
        // For each number in nums2, check if it exists in HashMap and has remaining count
        // Time complexity: O(m) where m is length of nums2
        for (int num : nums2) {
            int count = map.getOrDefault(num, 0);
            if (count > 0) {
                intersection.add(num);
                map.put(num, count - 1);  // Decrease count as we've used this number
            }
        }

        // Convert ArrayList to fixed-size array for return
        int[] result = new int[intersection.size()];
        // Third loop: Convert dynamic ArrayList to static array
        // Time complexity: O(k) where k is size of intersection
        for (int i = 0; i < intersection.size(); i++) {
            result[i] = intersection.get(i);
        }

        return result;
    }

    // Test cases
    public static void main(String[] args) {
        // Test case 1: Basic test with repeated elements
        int[] nums1 = {1, 2, 2, 1};
        int[] nums2 = {2, 2};
        System.out.println("Test case 1 result: " + Arrays.toString(intersect(nums1, nums2)));

        // Test case 2: Test with different array sizes and multiple occurrences
        int[] nums3 = {4, 9, 5};
        int[] nums4 = {9, 4, 9, 8, 4};
        System.out.println("Test case 2 result: " + Arrays.toString(intersect(nums3, nums4)));
    }
}

```





