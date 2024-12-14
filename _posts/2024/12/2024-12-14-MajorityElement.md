---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "169. Majority Element"
date: "2024-12-14"
tags: Easy
categories:
  - "LeetCode Array"
---

- Given an array `nums` of size `n`, return *the majority element*.
- The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

**Example 1**

```
Input: nums = [3,2,3]
Output: 3
```

**Example 2**

```
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2024/12/14
 */
public class MajorityElement {
    public static int majorityElement(int[] nums) {
        // Initialize the candidate for the majority element
        int candidate = nums[0];
        // Counter to track the "votes" for the candidate
        int count = 0;

        // First pass: Identify the majority candidate using Moore's Voting Algorithm
        for (int num : nums) {
            // If count reaches 0, we select the current number as a new candidate
            if (count == 0) {
                candidate = num;
            }
            // Update the count:
            // - If the current number equals the candidate, increase the count
            // - Otherwise, decrease the count
            count += (num == candidate) ? 1 : -1;
        }

        // Second pass: Verify that the candidate is indeed the majority element
        // This step is optional if the problem guarantees a majority element exists
        int verifyCount = 0;
        for (int num : nums) {
            if (num == candidate) {
                verifyCount++;
            }
        }

        // Optional verification: Uncomment if the problem does not guarantee a majority element
        // if (verifyCount > nums.length / 2) return candidate;

        // Return the majority candidate
        return candidate;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = {3, 2, 3};
        System.out.println("Majority Element: " + majorityElement(nums1)); // Output: 3

        // Test case 2
        int[] nums2 = {2, 2, 1, 1, 1, 2, 2};
        System.out.println("Majority Element: " + majorityElement(nums2)); // Output: 2
    }
}

```





