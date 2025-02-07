---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2799. Count Complete Subarrays in an Array"
date: "2025-02-05"
tags: Medium SlideWindow
categories:
  - "LeetCode DynamicSlidingWindowCountLongest"
---


- You are given an array `nums` consisting of **positive** integers.
- We call a subarray of an array **complete** if the following condition is satisfied:
  - The number of **distinct** elements in the subarray is equal to the number of distinct elements in the whole array.
- Return *the number of **complete** subarrays*.
- A **subarray** is a contiguous non-empty part of an array.

**Example 1**

```
Input: nums = [1,3,1,2,2]
Output: 4
Explanation: The complete subarrays are the following: [1,3,1,2], [1,3,1,2,2], [3,1,2] and [3,1,2,2].
```

**Example 2**

```
Input: nums = [5,5,5,5]
Output: 10
Explanation: The array consists only of the integer 5, so any subarray is complete. The number of subarrays that we can choose is 10.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindowCountLongest;

import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

/**
 * @author zhengxingxing
 * @date 2025/02/05
 */
public class CountCompleteSubarraysInAnArray {
    public static int countCompleteSubarrays(int[] nums) {
        // Step 1: Count distinct elements in the entire array using HashSet
        Set<Integer> total = new HashSet<>();
        for (int num : nums) {
            total.add(num);
        }
        // Store the total number of distinct elements
        int k = total.size();

        // Initialize variables for sliding window
        int ans = 0;          // Counter for valid complete subarrays
        int distinct = 0;     // Count of distinct elements in current window
        Map<Integer, Integer> count = new HashMap<>();  // Frequency map for elements in window
        int left = 0;         // Left pointer of sliding window

        // Iterate through array with right pointer
        for (int right = 0; right < nums.length; right++) {
            // Add current element to frequency map
            // If element exists, increment its count, if not, set count to 1
            count.put(nums[right], count.getOrDefault(nums[right], 0) + 1);

            // If this is the first occurrence of the element in window
            // increment distinct counter
            if (count.get(nums[right]) == 1) {
                distinct++;
            }

            // If window contains same number of distinct elements as whole array
            // start shrinking window from left
            while (distinct == k) {
                // Decrease frequency of leftmost element
                count.put(nums[left], count.get(nums[left]) - 1);

                // If element frequency becomes 0, it's no longer in window
                // so decrease distinct counter
                if (count.get(nums[left]) == 0) {
                    distinct--;
                }

                // Move left pointer forward
                left++;
            }

            // Add number of valid subarrays ending at current right pointer
            // left represents how many starting positions can form valid subarrays
            // with current right position as ending point
            ans += left;
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test case 1: Array with repeated elements
        int[] nums1 = {1, 3, 1, 2, 2};
        System.out.println("Result: " + countCompleteSubarrays(nums1));

        // Test case 2: Array with all same elements
        int[] nums2 = {5, 5, 5, 5};
        System.out.println("Result: " + countCompleteSubarrays(nums2));
    }
}

```





