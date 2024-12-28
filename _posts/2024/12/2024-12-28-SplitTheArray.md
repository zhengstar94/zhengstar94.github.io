---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3046. Split the Array"
date: "2024-12-28"
tags: Medium
categories:
  - "LeetCode Array"
---

- You are given an integer array `nums` of **even** length. You have to split the array into two parts `nums1` and `nums2` such that:
  - `nums1.length == nums2.length == nums.length / 2`.
  - `nums1` should contain **distinct** elements.
  - `nums2` should also contain **distinct** elements.
- Return `true` *if it is possible to split the array, and* `false` *otherwise**.*

**Example 1**

```
Input: nums = [1,1,2,2,3,4]
Output: true
Explanation: One of the possible ways to split nums is nums1 = [1,2,3] and nums2 = [1,2,4].
```

**Example 2**

```
Input: nums = [1,1,1,1]
Output: false
Explanation: The only possible way to split nums is nums1 = [1,1] and nums2 = [1,1]. Both nums1 and nums2 do not contain distinct elements. Therefore, we return false.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2024/12/28
 */
public class SplitTheArray {
    public static boolean isPossibleToSplit(int[] nums) {
        // Create counting array for numbers 1-100 (given constraint)
        int[] count = new int[101];

        // Count frequency of each number and check if any number appears more than twice
        for (int num: nums) {
            count[num]++;
            // If any number appears more than twice, it's impossible to split
            // because each part can contain at most one occurrence of each number
            if(count[num] > 2){
                return false;
            }
        }

        // If we reach here, no number appears more than twice
        // so we can split the array (one occurrence per part if needed)
        return true;
    }

    public static void main(String[] args) {
        // Test Case 1: Valid split possible
        // Can be split into [1,2,3] and [1,2,4]
        int[] nums1 = {1, 1, 2, 2, 3, 4};
        System.out.println("Test Case 1: " + Arrays.toString(nums1));
        System.out.println("Expected Result: true");
        System.out.println("Actual Result: " + isPossibleToSplit(nums1));
        System.out.println();

        // Test Case 2: Invalid case - number appears more than twice
        // Cannot be split as '1' appears four times
        int[] nums2 = {1, 1, 1, 1};
        System.out.println("Test Case 2: " + Arrays.toString(nums2));
        System.out.println("Expected Result: false");
        System.out.println("Actual Result: " + isPossibleToSplit(nums2));
        System.out.println();

        // Test Case 3: Edge case - all distinct elements
        // Can be split into [1,2] and [3,4]
        int[] nums3 = {1, 2, 3, 4};
        System.out.println("Test Case 3: " + Arrays.toString(nums3));
        System.out.println("Expected Result: true");
        System.out.println("Actual Result: " + isPossibleToSplit(nums3));
    }
}

```





