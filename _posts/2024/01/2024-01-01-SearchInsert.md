---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "35.Search Insert"
date: "2024-01-01"
categories:
  - "LeetCode BinarySearch"
---

# LeetCode 35. Search Insert[Easy]

- Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
- You may assume no duplicates in the array.

**Example 1**

```
Input: [1,3,5,6], 5
Output: 2
```

**Example 2**

```
Input: [1,3,5,6], 2
Output: 1
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
public class SearchInsertPosition {
    public static int searchInsert(int[] array, int n) {
        int left = 0;
        int right = array.length;

        while (left < right){
            int mid = left + (right - left ) / 2;
            if(array[mid] == n){
                return mid;
            }else if(array[mid] > n){
                right  = mid;
            }else{
                left = mid + 1;
            }
        }

        return left;
    }
  
  public static int searchInsert2(int[] array, int n) {
        int left = 0;
        int right = array.length - 1;

        while (left <= right){
            int mid = left + (right - left ) / 2;
            if(array[mid] == n){
                return mid;
            }else if(array[mid] > n){
                right  = mid - 1;
            }else{
                left = mid + 1;
            }
        }

        return left;
    }

    public static void main(String[] args) {
        System.out.println(searchInsert(new int[]{1,3,5,7},0));
    }
}

```








