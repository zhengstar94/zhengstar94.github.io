---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "26. Remove Duplicates from Sorted Array"
date: "2024-11-14"
categories:
  - "LeetCode TwoPointers"
---

- Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**. Then return *the number of unique elements in* `nums`.

- Consider the number of unique elements of `nums` to be `k`, to get accepted, you need to do the following things:

  - Change the array `nums` such that the first `k` elements of `nums` contain the unique elements in the order they were present in `nums` initially. The remaining elements of `nums` are not important as well as the size of `nums`.
  - Return `k`.

- **Custom Judge:**

  The judge will test your solution with the following code:

  ```
  int[] nums = [...]; // Input array
  int[] expectedNums = [...]; // The expected answer with correct length
  
  int k = removeDuplicates(nums); // Calls your implementation
  
  assert k == expectedNums.length;
  for (int i = 0; i < k; i++) {
      assert nums[i] == expectedNums[i];
  }
  ```

  If all assertions pass, then your solution will be **accepted**.



**Example 1**

```
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Example 2**

```
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2024/11/14
 */
public class RemoveDuplicatesFromSortedArray {
    public static int removeDuplicates(int[] nums) {
        // If the array is null or has no elements, return 0 as there are no elements to process
        if (nums == null || nums.length == 0) {
            return 0;
        }

        // Initialize a variable k to track the position of the next unique element; start from 1 as the first element is always unique
        int k = 1;

        // Loop through the array starting from the second element
        for (int i = 1; i < nums.length; i++) {
            // If the current element is not equal to the previous one, it is unique
            if (nums[i] != nums[i - 1]) {
                // Place the unique element at the position k, then increment k
                nums[k++] = nums[i];
            }
        }

        // Return the count of unique elements, which is stored in k
        return k;
    }

    public static void main(String[] args) {

        // Test array with sorted elements including duplicates
        int[] nums = {0, 0, 1, 1, 1, 2, 2, 3, 3, 4};

        // Call removeDuplicates to get the count of unique elements and modify the array in place
        int k = removeDuplicates(nums);

        System.out.println("Unique count: " + k);
        System.out.print("Modified array: ");
        for (int i = 0; i < k; i++) {
            System.out.print(nums[i] + " ");
        }
    }
}

```
