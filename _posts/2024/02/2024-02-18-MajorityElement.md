---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "169.Majority Element"
date: "2024-02-18"
categories:
  - "LeetCode HashTable"
---

# LeetCode 169. Majority Element

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
【O(n)time∣O(1)space】
```

```java
package Leetcode.HashTable;

/**
 * @author zhengstars
 * @date 2024/02/24
 */
public class MajorityElement {
    public static int majorityElement(int[] nums) {
        // Initialize the counter to 0
        int count = 0;
        // Initialize the candidate to null
        Integer candidate = null;

        // Iterate through the array
        for (int num : nums) {
            // Choose the current element as the candidate when the counter is 0
            if (count == 0) {
                candidate = num;
            }
            // If the element being traversed is the same as the candidate, increment the counter by 1; otherwise, decrement the counter by 1
            count += (num == candidate) ? 1 : -1;
        }

        // Return the final candidate
        return candidate;
    }
    
    public static void main(String[] args) {
        // Test array 1
        int[] nums1 = {3, 2, 3};
        // Test array 2
        int[] nums2 = {2, 2, 1, 1, 1, 2, 2};
        // Print the majority element of nums1
        System.out.println("The majority element of nums1 is: " + majorityElement(nums1));
        // Print the majority element of nums2
        System.out.println("The majority element of nums2 is: " + majorityElement(nums2));
    }
}
```

