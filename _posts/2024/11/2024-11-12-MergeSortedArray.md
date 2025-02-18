---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "88.Merge Sorted Array"
date: "2024-11-12"
categories:
  - "LeetCode TwoPointers"
---

- You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.
- **Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.
- The final sorted array should not be returned by the function, but instead be *stored inside the array* `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example 1**

```
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
```

**Example 2**

```
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].
```

**Example 3**

```
Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.
```

## Method 1

```tex
【O(m+n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2024/11/12
 */
public class MergeSortedArray {
    public static void merge(int[] nums1, int m, int[] nums2, int n) {
        // Initialize pointers:
        // p1 points to the last valid element in nums1
        int p1 = m - 1;
        // p2 points to the last element in nums2
        int p2 = n - 1;
        // p points to the last position in the merged array
        int p = m + n - 1;

        // Compare elements from both arrays and place them in correct position
        while (p1 >= 0 && p2 >= 0) {
            if (nums1[p1] > nums2[p2]) {
                // If element from nums1 is larger, place it at the end
                nums1[p] = nums1[p1];
                p1--;
            } else {
                // If element from nums2 is larger or equal, place it at the end
                nums1[p] = nums2[p2];
                p2--;
            }
            // Move the merger pointer one position back
            p--;
        }

        // If there are remaining elements in nums2, copy them to the beginning of nums1
        // Note: if p1 >= 0, no action needed as elements are already in place
        System.arraycopy(nums2, 0, nums1, 0, p2 + 1);
    }

    public static void main(String[] args) {
        // Test Case 1: Regular case with equal length arrays
        int[] nums1 = {1, 2, 3, 0, 0, 0};  // Array with extra space at end
        int[] nums2 = {2, 5, 6};
        int m = 3;  // Number of elements in nums1
        int n = 3;  // Number of elements in nums2

        System.out.println("Test Case 1:");
        System.out.print("Before merge nums1: ");
        printArray(nums1);
        System.out.print("nums2: ");
        printArray(nums2);

        merge(nums1, m, nums2, n);

        System.out.print("After merge: ");
        printArray(nums1);
        System.out.println();

        // Test Case 2: Edge case where second array is empty
        int[] nums3 = {1};
        int[] nums4 = {};

        System.out.println("Test Case 2:");
        System.out.print("Before merge nums1: ");
        printArray(nums3);
        System.out.print("nums2: ");
        printArray(nums4);

        merge(nums3, 1, nums4, 0);

        System.out.print("After merge: ");
        printArray(nums3);
        System.out.println();

        // Test Case 3: Edge case where first array is empty
        int[] nums5 = {0};
        int[] nums6 = {1};

        System.out.println("Test Case 3:");
        System.out.print("Before merge nums1: ");
        printArray(nums5);
        System.out.print("nums2: ");
        printArray(nums6);

        merge(nums5, 0, nums6, 1);

        System.out.print("After merge: ");
        printArray(nums5);
    }

    /**
     * Helper method to print array contents in a formatted way
     *
     * @param arr The array to be printed
     */
    private static void printArray(int[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) {
                System.out.print(", ");
            }
        }
        System.out.println("]");
    }
}
```





