---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "349.Intersection of Two Arrays"
date: "2024-10-16"
categories:
  - "LeetCode Array"
---

# 349. Intersection of Two Arrays

- Given two integer arrays `nums1` and `nums2`, return *an array of their intersection*. Each element in the result must be **unique** and you may return the result in **any order**.


**Example 1**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

**Example 2**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
Explanation: [4,9] is also accepted.
```

## Method 1

```tex
【O(n + m) time | O(min(n, m)) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;

/**
 * @author zhengstars
 * @date 2024/10/16
 */
public class IntersectionOfTwoArrays {
    public static int[] intersection(int[] nums1, int[] nums2) {
        // Create a HashSet to store unique elements from nums1
        Set<Integer> set1 = new HashSet<>();
        // Create a HashSet to store the intersection elements
        Set<Integer> resultSet = new HashSet<>();

        // Add all elements from nums1 to set1 (automatically removes duplicates)
        for (int num: nums1) {
            set1.add(num);
        }

        // Check each element in nums2
        // If it exists in set1, it's an intersection element
        for (int num: nums2) {
            if(set1.contains(num)){
                resultSet.add(num);
            }
        }

        // Convert HashSet to array for return value
        int[] result = new int[resultSet.size()];
        int i = 0;
        for (int num: resultSet) {
            result[i++] = num;
        }

        return result;
    }

    /**
     * Main method containing test cases
     * @param args command line arguments (not used)
     */
    public static void main(String[] args) {
        // Test Case 1: Basic test with duplicates
        // Expected output: [2]
        int[] nums1 = {1, 2, 2, 1};
        int[] nums2 = {2, 2};
        int[] result1 = intersection(nums1, nums2);
        System.out.println("Test Case 1:");
        System.out.println("Input: nums1 = [1,2,2,1], nums2 = [2,2]");
        System.out.println("Output: " + Arrays.toString(result1));

        // Test Case 2: No intersection
        // Expected output: []
        int[] nums3 = {1, 2, 3};
        int[] nums4 = {4, 5, 6};
        int[] result2 = intersection(nums3, nums4);
        System.out.println("Test Case 2:");
        System.out.println("Input: nums1 = [1,2,3], nums2 = [4,5,6]");
        System.out.println("Output: " + Arrays.toString(result2));

        // Test Case 3: Multiple intersections
        // Expected output: [4,9]
        int[] nums5 = {4, 9, 5};
        int[] nums6 = {9, 4, 9, 8, 4};
        int[] result3 = intersection(nums5, nums6);
        System.out.println("Test Case 3:");
        System.out.println("Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]");
        System.out.println("Output: " + Arrays.toString(result3));

        // Test Case 4: Empty array
        // Expected output: []
        int[] nums7 = {};
        int[] nums8 = {1, 2, 3};
        int[] result4 = intersection(nums7, nums8);
        System.out.println("Test Case 4:");
        System.out.println("Input: nums1 = [], nums2 = [1,2,3]");
        System.out.println("Output: " + Arrays.toString(result4));

        // Test Case 5: Identical arrays
        // Expected output: [1,2,3]
        int[] nums9 = {1, 2, 3};
        int[] nums10 = {1, 2, 3};
        int[] result5 = intersection(nums9, nums10);
        System.out.println("Test Case 5:");
        System.out.println("Input: nums1 = [1,2,3], nums2 = [1,2,3]");
        System.out.println("Output: " + Arrays.toString(result5));
    }
}

```





