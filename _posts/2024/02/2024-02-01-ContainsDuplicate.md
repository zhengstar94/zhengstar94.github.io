---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "217.Contains Duplicate"
date: "2024-02-01"
categories:
  - "LeetCode Array"
---

# LeetCode 217. Contains Duplicate [Easy]

- Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

```
Input: nums = [1,2,3,1]
Output: true
```

**Example 2**

```
Input: nums = [1,2,3,4]
Output: false
```

**Example 3**

```
Input: nums = [1,1,1,3,3,4,3,2,4,2]
Output: true
```

## Method 1

```tex
【O(n)time∣O(n)space】
```

```java
package Leetcode.Array;

import java.util.HashSet;
import java.util.Set;

/**
 * @author zhengstars
 * @date 2024/02/19
 */
public class ContainsDuplicate {
    public static boolean containsDuplicate(int[] nums) {
        // Create a new HashSet instance
        Set<Integer> set = new HashSet<Integer>();

        // Iterate over the given array
        for (int x : nums) {
            // Try to add each element to the set
            if (!set.add(x)) {
                // If set.add() returns false, it means the element already exists in the set (duplicate found)
                return true;
            }
        }
        // If no duplicate is found after the iteration, return false
        return false;
    }

    /**
     * The main function to test the solution
     */
    public static void main(String[] args) {

        // Test case 1. Expected output: true
        int[] nums1 = {1,2,3,1};
        System.out.println(containsDuplicate(nums1));

        // Test case 2. Expected output: false
        int[] nums2 = {1,2,3,4};
        System.out.println(containsDuplicate(nums2));

        // Test case 3. Expected output: true
        int[] nums3 = {1,1,1,3,3,4,3,2,4,2};
        System.out.println(containsDuplicate(nums3));
    }
}

```

