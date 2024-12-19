---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "769. Max Chunks To Make Sorted"
date: "2024-12-19"
tags: Medium
categories:
  - "LeetCode Array"
---


- You are given an integer array `arr` of length `n` that represents a permutation of the integers in the range `[0, n - 1]`.
- We split `arr` into some number of **chunks** (i.e., partitions), and individually sort each chunk. After concatenating them, the result should equal the sorted array.
- Return *the largest number of chunks we can make to sort the array*.


**Example 1**

```
Input: arr = [4,3,2,1,0]
Output: 1
Explanation:
Splitting into two or more chunks will not return the required result.
For example, splitting into [4, 3], [2, 1, 0] will result in [3, 4, 0, 1, 2], which isn't sorted.
```

**Example 2**

```
Input: arr = [1,0,2,3,4]
Output: 4
Explanation:
We can split into two chunks, such as [1, 0], [2, 3, 4].
However, splitting into [1, 0], [2], [3], [4] is the highest number of chunks possible.
```

## Method 1

```tex
【O(n) time | O(k) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2024/12/19
 */
public class MaxChunksToMakeSorted {
    public static int maxChunksToSorted(int[] arr) {
        int chunks = 0;      // Counter for number of valid chunks
        int currentMax = 0;  // Tracks maximum value seen so far

        // Iterate through the array
        // Key insight: if the maximum value seen equals current index,
        // all numbers before this position will be smaller and can form a chunk
        for (int i = 0; i < arr.length; i++) {
            currentMax = Math.max(currentMax, arr[i]);

            // If currentMax equals current index, we can form a valid chunk
            // Because all numbers in current chunk will be in their correct positions after sorting
            if(currentMax == i){
                chunks++;
            }
        }
        return chunks;
    }
    public static void main(String[] args) {
        // Test Case 1: Array that can only be split into 1 chunk
        int[] arr1 = {4,3,2,1,0};
        System.out.println("Test Case 1:");
        System.out.println("Input: " + java.util.Arrays.toString(arr1));
        System.out.println("Output: " + maxChunksToSorted(arr1));
        System.out.println("Expected: 1");
        System.out.println();

        // Test Case 2: Array that can be split into 4 chunks
        int[] arr2 = {1,0,2,3,4};
        System.out.println("Test Case 2:");
        System.out.println("Input: " + java.util.Arrays.toString(arr2));
        System.out.println("Output: " + maxChunksToSorted(arr2));
        System.out.println("Expected: 4");
        System.out.println();

        // Test Case 3: Already sorted array (maximum possible chunks)
        int[] arr3 = {0,1,2,3,4};
        System.out.println("Test Case 3:");
        System.out.println("Input: " + java.util.Arrays.toString(arr3));
        System.out.println("Output: " + maxChunksToSorted(arr3));
        System.out.println("Expected: 5");
    }
}

```





