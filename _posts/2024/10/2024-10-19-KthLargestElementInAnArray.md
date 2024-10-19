---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "215.Kth Largest Element in an Array"
date: "2024-10-19"
categories:
  - "LeetCode Array"
---


- Given an integer array `nums` and an integer `k`, return *the* `kth` *largest element in the array*.
- Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.
- Can you solve it without sorting?


**Example 1**

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

**Example 2**

```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4
```

## Method 1

```tex
【O(nlog(k)) time | O(k) space】
```

```java
package Leetcode.Array;

import java.util.PriorityQueue;

/**
 * @author zhengstars
 * @date 2024/10/19
 */
public class KthLargestElementInAnArray {
    public static int findKthLargest(int[] nums, int k) {
        // Initialize a min-heap (PriorityQueue by default is a min-heap)
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();

        // Iterate through all the elements in the array
        for (int num : nums) {
            // Add the current number to the min-heap
            minHeap.add(num);

            // If the heap size exceeds k, remove the smallest element from the heap
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }

        // The top element of the heap is the kth largest element
        return minHeap.peek();
    }

    public static void main(String[] args) {
        // Test case 1
        int[] nums1 = { 3, 2, 1, 5, 6, 4 };
        int k1 = 2;
        System.out.println("Test Case 1: " + findKthLargest(nums1, k1)); // Output: 5

        // Test case 2
        int[] nums2 = { 3, 2, 3, 1, 2, 4, 5, 5, 6 };
        int k2 = 4;
        System.out.println("Test Case 2: " + findKthLargest(nums2, k2)); // Output: 4

        // Test case 3
        int[] nums3 = { 7, 10, 4, 3, 20, 15 };
        int k3 = 3;
        System.out.println("Test Case 3: " + findKthLargest(nums3, k3)); // Output: 10

        // Test case 4
        int[] nums4 = { 1, 1, 1, 1, 1, 1 };
        int k4 = 1;
        System.out.println("Test Case 4: " + findKthLargest(nums4, k4)); // Output: 1

        // Test case 5 (single element)
        int[] nums5 = { 99 };
        int k5 = 1;
        System.out.println("Test Case 5: " + findKthLargest(nums5, k5)); // Output: 99
    }
}

```





