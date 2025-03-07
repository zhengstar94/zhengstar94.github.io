---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "80. Remove Duplicates from Sorted Array II"
date: "2025-02-09"
tags: Medium TwoPointers
categories:
  - "LeetCode SingleSeqInPlacePointers"
---

- Given an integer array `nums` sorted in **non-decreasing order**, remove some duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears **at most twice**. The **relative order** of the elements should be kept the **same**.

- Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

- Return `k` *after placing the final result in the first* `k` *slots of* `nums`.

- Do **not** allocate extra space for another array. You must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

- **Custom Judge:**

- The judge will test your solution with the following code:

  - > ```
    > int[] nums = [...]; // Input array
    > int[] expectedNums = [...]; // The expected answer with correct length
    > 
    > int k = removeDuplicates(nums); // Calls your implementation
    > 
    > assert k == expectedNums.length;
    > for (int i = 0; i < k; i++) {
    >     assert nums[i] == expectedNums[i];
    > }
    > ```

- If all assertions pass, then your solution will be **accepted**.

**Example 1**

```
Input: nums = [1,1,1,2,2,3]
Output: 5, nums = [1,1,2,2,3,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.
It does not matter what you leave beyond  the returned k (hence they are underscores).
```

**Example 2**

```
Input: nums = [0,0,1,1,1,1,2,3,3]
Output: 7, nums = [0,0,1,1,2,3,3,_,_]
Explanation: Your function should return k = 7, with the first seven elements of nums being 0, 0, 1, 1, 2, 3 and 3 respectively.
It does not matter what you leave beyond  the returned k (hence they are underscores).
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/02/09
 */
public class RemoveDuplicatesFromSortedArrayII {

    public static int removeDuplicates(int[] nums) {
        // If the array has less than or equal to 2 elements, no need to remove any duplicates.
        // All elements can be kept, as they satisfy the condition of appearing at most twice.
        if (nums.length <= 2) {
            return nums.length;
        }

        // Two-pointer technique:
        // i acts as the pointer for the new, filtered array where elements are written.
        // Initialize i to 2 because the first two elements are always kept (no extra duplicates to remove).
        int i = 2;

        // Iterate through the array starting from the 3rd element (index 2).
        for (int j = 2; j < nums.length; j++) {
            // nums[j] is the current element being checked.
            // nums[i - 2] is the element at the position 2 steps behind the last written position i.
            // If nums[j] != nums[i - 2], it means nums[j] has not yet appeared more than twice.
            // This allows us to keep nums[j] in the final array.
            if (nums[j] != nums[i - 2]) {
                // Copy nums[j] to the current position i, effectively "keeping" this element.
                nums[i] = nums[j];
                // Increment i, moving to the next position to potentially write to in the filtered array.
                i++;
            }
            // If nums[j] == nums[i - 2], it means nums[j] has already been included twice,
            // so we skip it and do not increment i.
        }

        // Return the new length of the array. The elements from nums[0] to nums[i - 1]
        // are the elements that satisfy the condition of appearing at most twice.
        return i;
    }

    public static void main(String[] args) {
        // Example 1: Input array has multiple duplicates (more than twice).
        int[] nums1 = {1, 1, 1, 2, 2, 3};
        int k1 = removeDuplicates(nums1); // Call the method to remove duplicates.
        System.out.print("New length: " + k1 + ", Array: ");
        printArray(nums1, k1); // Print the modified array up to the new length.

        // Example 2: Input array has a mixture of duplicates and unique elements.
        int[] nums2 = {0, 0, 1, 1, 1, 1, 2, 3, 3};
        int k2 = removeDuplicates(nums2); // Call the method to remove duplicates.
        System.out.print("New length: " + k2 + ", Array: ");
        printArray(nums2, k2); // Print the modified array up to the new length.
    }


    private static void printArray(int[] nums, int length) {
        // Print each of the first `length` elements of the array.
        for (int i = 0; i < length; i++) {
            System.out.print(nums[i] + " ");
        }
        // Print a new line after printing all required elements.
        System.out.println();
    }
}

```






