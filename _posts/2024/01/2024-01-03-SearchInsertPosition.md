---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "34.Search Insert Position"
date: "2024-01-03"
categories:
  - "LeetCode Binary Search"
---

# LeetCode 34. Search Insert Position [Easy]

- Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.
- Your algorithm’s runtime complexity must be in the order of O(log n).
- If the target is not found in the array, return [-1, -1].

**Example 1**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```



## Method 1

```tex
【O(log(n))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2024/01/14
 */
public class SearchRange {
    public int[] searchRange(int[] nums, int target) {
        return new int[]{firstPos(nums, target), lastPos(nums, target)};
    }

    private int firstPos(int[] nums, int target) {
        int l = 0, r = nums.length;
        while (l < r) {
            int m = l + (r - l) / 2;
            if (nums[m] >= target) {
                r = m;
            } else {
                l = m + 1;
            }
        }
        if (l == nums.length || nums[l] != target) {
            return -1;
        }
        return l;
    }

    private int lastPos(int[] nums, int target) {
        int l = 0, r = nums.length;
        while (l < r) {
            int m = l + (r - l) / 2;
            if (nums[m] > target) {
                r = m;
            } else {
                l = m + 1;
            }
        }
        l--;
        if (l < 0 || nums[l] != target) {
            return -1;
        }
        return l;
    }
}

```















