---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "704.Binary Search"
date: "2024-01-02"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 704. Binary Search [Easy]

- Given a **sorted** (in ascending order) integer array `nums` of `n` elements and a `target` value, write a function to search `target` in `nums`. If `target` exists, then return its index, otherwise return `-1`.

**Example 1**

```
Input: nums = [-1,0,3,5,9,12], target = 9 Output: 4 Explanation: 9 exists in nums and its index is 4
```

**Example 2**

```
Input: nums = [-1,0,3,5,9,12], target = 2 Output: -1 Explanation: 2 does not exist in nums so return -1
```



## Method 1

```tex
【O(log(n))time∣O(1)space】
```

```java
package Leetcode.BinarySearch;

/**
 * @author zhengstars
 * @date 2024/01/21
 */
public class BinarySearch {
    public static int searchInsert(int[] array, int target) {
        int left = 0;
        int right = array.length;

        while (left < right){
            int mid = left + (right - left)/2;

            if(array[mid] == target){
                return mid;
            }if(array[mid] > target){
                right = mid;
            }else{
                left = mid + 1;
            }
        }
        return -1;
    }
}

```















