---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "31.Next Permutation"
date: "2024-10-30"
categories:
  - "LeetCode Array"
---

- A **permutation** of an array of integers is an arrangement of its members into a sequence or linear order.
  - For example, for `arr = [1,2,3]`, the following are all the permutations of `arr`: `[1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1]`.
- The **next permutation** of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the **next permutation** of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).
  - For example, the next permutation of `arr = [1,2,3]` is `[1,3,2]`.
  - Similarly, the next permutation of `arr = [2,3,1]` is `[3,1,2]`.
  - While the next permutation of `arr = [3,2,1]` is `[1,2,3]` because `[3,2,1]` does not have a lexicographical larger rearrangement.
- Given an array of integers `nums`, *find the next permutation of* `nums`.
- The replacement must be **[in place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

**Example 1**

```
Input: nums = [1,2,3]
Output: [1,3,2]
```

**Example 2**

```
Input: nums = [3,2,1]
Output: [1,2,3]
```

**Example 3**

```
Input: nums = [1,1,5]
Output: [1,5,1]
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengstars
 * @date 2024/10/30
 */
public class NextPermutation {
    public static void nextPermutation(int[] nums) {
        // 1. Boundary check - return if array is null or has length <= 1
        if (nums == null || nums.length <= 1) {
            return;
        }

        int n = nums.length;
        int i = n - 2;

        // 2. Find first decreasing element from right
        // Example: in [1,5,8,4,7,6,5,3,1], find 4 because 4 < 7
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            i--;
        }

        // 3. If we found a decreasing element (i.e., i >= 0)
        if (i >= 0) {
            int j = n - 1;
            // Find the smallest element from right that's greater than nums[i]
            // This element will exist because nums[i] < nums[i+1]
            while (j > i && nums[j] <= nums[i]) {
                j--;
            }
            // Swap nums[i] with this element
            swap(nums, i, j);
        }

        // 4. Reverse all elements after position i to get the smallest permutation
        // This works because elements after i are in descending order
        reverse(nums, i + 1, n - 1);
    }

    private static void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

    private static void reverse(int[] nums, int start, int end) {
        while (start < end) {
            swap(nums, start++, end--);
        }
    }
    
    public static void main(String[] args) {
        // Test Case 1: Normal case
        int[] test1 = {1, 2, 3};
        nextPermutation(test1);
        System.out.println("Test 1: [ 1,2,3 ] -> " + arrayToString(test1) + " (Expected: [ 1,3,2 ] )");

        // Test Case 2: Decreasing order - should return increasing order
        int[] test2 = {3, 2, 1};
        nextPermutation(test2);
        System.out.println("Test 2: [ 3,2,1 ] -> " + arrayToString(test2) + " (Expected: [ 1,2,3 ] )");

        // Test Case 3: Array with duplicates
        int[] test3 = {1, 1, 5};
        nextPermutation(test3);
        System.out.println("Test 3: [ 1,1,5 ] -> " + arrayToString(test3) + " (Expected: [ 1,5,1 ] )");

        // Test Case 4: Larger array
        int[] test4 = {1, 3, 5, 4, 2};
        nextPermutation(test4);
        System.out.println("Test 4: [ 1,3,5,4,2 ] -> " + arrayToString(test4) + " (Expected: [ 1,4,2,3,5 ] )");
    }


    private static String arrayToString(int[] arr) {
        if (arr == null){
            return "null";
        }
        StringBuilder sb = new StringBuilder("[");
        for (int i = 0; i < arr.length; i++) {
            sb.append(arr[i]);
            if (i < arr.length - 1) {
                sb.append(",");
            }
        }
        sb.append("]");
        return sb.toString();
    }
    
}

```





