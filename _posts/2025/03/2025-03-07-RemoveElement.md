---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "27. Remove Element"
date: "2025-03-07"
tags: Medium TwoPointers
categories:
  - "LeetCode SingleSeqInPlacePointers" 
---


- Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm). The order of the elements may be changed. Then return *the number of elements in* `nums` *which are not equal to* `val`.

- Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to do the following things:

  - Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
  - Return `k`.

- **Custom Judge:**

- The judge will test your solution with the following code:

  ```
  int[] nums = [...]; // Input array
  int val = ...; // Value to remove
  int[] expectedNums = [...]; // The expected answer with correct length.
                              // It is sorted with no values equaling val.
  
  int k = removeElement(nums, val); // Calls your implementation
  
  assert k == expectedNums.length;
  sort(nums, 0, k); // Sort the first k elements of nums
  for (int i = 0; i < actualLength; i++) {
      assert nums[i] == expectedNums[i];
  }
  ```

- If all assertions pass, then your solution will be **accepted**.

**Example 1**

```
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Example 2**

```
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer.SingleSeqInPlacePointers;

/**
 * @author zhengxingxing
 * @date 2025/03/07
 */
public class RemoveElement {

    public static int removeElement(int[] nums, int val) {
        // Initialize two pointers
        // slowIndex: points to the position where next valid element should be placed
        // fastIndex: scans through the array to find valid elements
        int slowIndex = 0;
        int fastIndex = 0;

        // Iterate through the array using fast pointer
        while (fastIndex < nums.length) {
            // If current element is not equal to val
            // Copy it to the position indicated by slow pointer
            if (nums[fastIndex] != val) {
                nums[slowIndex] = nums[fastIndex];
                // Move slow pointer to next position
                slowIndex++;
            }
            // Always move fast pointer to scan next element
            fastIndex++;
        }

        // Return the length of new array (indicated by slow pointer)
        return slowIndex;
    }
    
    private static void printArray(int[] nums, int length) {
        System.out.print("[");
        for (int i = 0; i < length; i++) {
            System.out.print(nums[i]);
            if (i < length - 1) {
                System.out.print(", ");
            }
        }
        System.out.println("]");
    }

    public static void main(String[] args) {
        // Test Case 1: Regular case with repeated elements
        int[] nums1 = {3, 2, 2, 3};
        int val1 = 3;
        System.out.println("Test Case 1:");
        System.out.print("Original Array: ");
        printArray(nums1, nums1.length);
        int result1 = removeElement(nums1, val1);
        System.out.print("Result Array: ");
        printArray(nums1, result1);
        System.out.println("New Length: " + result1);
        System.out.println();

        // Test Case 2: Larger array with multiple instances of target value
        int[] nums2 = {0,1,2,2,3,0,4,2};
        int val2 = 2;
        System.out.println("Test Case 2:");
        System.out.print("Original Array: ");
        printArray(nums2, nums2.length);
        int result2 = removeElement(nums2, val2);
        System.out.print("Result Array: ");
        printArray(nums2, result2);
        System.out.println("New Length: " + result2);
        System.out.println();

        // Test Case 3: Edge case - empty array
        int[] nums3 = {};
        int val3 = 1;
        System.out.println("Test Case 3:");
        System.out.print("Original Array: ");
        printArray(nums3, nums3.length);
        int result3 = removeElement(nums3, val3);
        System.out.print("Result Array: ");
        printArray(nums3, result3);
        System.out.println("New Length: " + result3);
    }
}

```





