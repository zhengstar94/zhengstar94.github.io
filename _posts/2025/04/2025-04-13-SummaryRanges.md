---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "228. Summary Ranges"
date: "2025-04-13"
tags: Easy
categories:
  - "LeetCode GroupedLoop"
---


- You are given a **sorted unique** integer array `nums`.
- A **range** `[a,b]` is the set of all integers from `a` to `b` (inclusive).
- Return *the **smallest sorted** list of ranges that **cover all the numbers in the array exactly***. That is, each element of `nums` is covered by exactly one of the ranges, and there is no integer `x` such that `x` is in one of the ranges but not in `nums`.
- Each range `[a,b]` in the list should be output as:
  - `"a->b"` if `a != b`
  - `"a"` if `a == b`

**Example 1**

```
Input: nums = [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: The ranges are:
[0,2] --> "0->2"
[4,5] --> "4->5"
[7,7] --> "7"
```

**Example 2**

```
Input: nums = [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: The ranges are:
[0,0] --> "0"
[2,4] --> "2->4"
[6,6] --> "6"
[8,9] --> "8->9"
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.GroupedLoop;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2025/04/13
 */
public class SummaryRanges {
    
    public static List<String> summaryRanges(int[] nums) {
        // Initialize result list to store range strings
        List<String> result = new ArrayList<>();

        // Handle edge cases: null array or empty array
        if (nums == null || nums.length == 0){
            return result;
        }

        // Get array length for iteration
        int n = nums.length;
        // Initialize index for traversal
        int i = 0;

        // Main loop to process the entire array
        while (i < n){
            // Mark the start of current group/range
            int start = i;

            // Inner loop to find the end of current continuous sequence
            // Continue while next number is consecutive
            while (i + 1 < n && nums[i + 1] == nums[i] + 1){
                i++;
            }

            // Process the found range
            if (start == i){
                // Case 1: Single number range (start equals end)
                // Format: "number"
                result.add(String.valueOf(nums[start]));
            }else{
                // Case 2: Multiple number range (start to end)
                // Format: "start->end"
                result.add(nums[start] + "->" + nums[i]);
            }
            // Move to the start of next potential range
            i++;
        }

        return result;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Regular case with multiple ranges
        int[] nums1 = {0, 1, 2, 4, 5, 7};
        System.out.println("Test Case 1 Result: " + summaryRanges(nums1));
        // Expected output: ["0->2","4->5","7"]

        // Test Case 2: Mixed single numbers and ranges
        int[] nums2 = {0, 2, 3, 4, 6, 8, 9};
        System.out.println("Test Case 2 Result: " + summaryRanges(nums2));
        // Expected output: ["0","2->4","6","8->9"]

        // Test Case 3: Empty array edge case
        int[] nums3 = {};
        System.out.println("Test Case 3 Result: " + summaryRanges(nums3));
        // Expected output: []

        // Test Case 4: Single element array
        int[] nums4 = {1};
        System.out.println("Test Case 4 Result: " + summaryRanges(nums4));
        // Expected output: ["1"]
    }
}

```





