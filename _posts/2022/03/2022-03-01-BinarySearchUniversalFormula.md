---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "Binary Search Universal Formula"
date: "2022-03-01"
categories: 
  - "Data Structure"
---

# Binary Search(Universal Formula)

## Pattern 1: Basic template

```java
/* Pattern 1: Search for any element that meets the condition, tighten the boundary to the right-side */
public int binarySearchPattern1(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            return mid;
        }
    }
    // Return -1 indicates not found
    return -1;
}
```



## Pattern 2: Find the first location equal to the target

```java
/* Pattern 2: Search for the leftmost element that meets the condition, tighten left-side boundary */

public int binarySearchPattern2(int[] nums, int target) {
    int left = 0, right = nums.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    return left;
}
```

## Pattern 3: Find the last position equal to the target

```java
/* Pattern 3: Search for the rightmost element that meets the condition, result just meets the conditions */

public int binarySearchPattern3(int[] nums, int target) {
    int left = 0, right = nums.length;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] <= target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    return right - 1;
}
```

