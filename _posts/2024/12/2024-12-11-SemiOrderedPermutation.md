---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2717. Semi-Ordered Permutation"
date: "2024-12-11"
tags: Easy
categories:
  - "LeetCode Array"
---


- ou are given a **0-indexed** permutation of `n` integers `nums`.
- A permutation is called **semi-ordered** if the first number equals `1` and the last number equals `n`. You can perform the below operation as many times as you want until you make `nums` a **semi-ordered** permutation:
  - Pick two adjacent elements in `nums`, then swap them.
- Return *the minimum number of operations to make* `nums` *a **semi-ordered permutation***.
- A **permutation** is a sequence of integers from `1` to `n` of length `n` containing each number exactly once.

**Example 1**

```
Input: nums = [2,1,4,3]
Output: 2
Explanation: We can make the permutation semi-ordered using these sequence of operations: 
1 - swap i = 0 and j = 1. The permutation becomes [1,2,4,3].
2 - swap i = 2 and j = 3. The permutation becomes [1,2,3,4].
It can be proved that there is no sequence of less than two operations that make nums a semi-ordered permutation. 
```

**Example 2**

```
Input: nums = [2,4,1,3]
Output: 3
Explanation: We can make the permutation semi-ordered using these sequence of operations:
1 - swap i = 1 and j = 2. The permutation becomes [2,1,4,3].
2 - swap i = 0 and j = 1. The permutation becomes [1,2,4,3].
3 - swap i = 2 and j = 3. The permutation becomes [1,2,3,4].
It can be proved that there is no sequence of less than three operations that make nums a semi-ordered permutation.
```

**Example 3**

```
Input: nums = [1,3,4,2,5]
Output: 0
Explanation: The permutation is already a semi-ordered permutation.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2024/12/11
 */
public class SemiOrderedPermutation {
    public static int semiOrderedPermutation(int[] nums) {
        // Get the length of the input array
        int n = nums.length;

        // Initialize indices for numbers 1 and n
        // Set to -1 to indicate not found initially
        int oneIndex = -1;
        int nIndex = -1;

        // Find the positions of numbers 1 and n in the array
        for (int i = 0; i < n; i++) {
            // Record the index of number 1
            if (nums[i] == 1){
                oneIndex = i;
            }

            // Record the index of number n
            if (nums[i] == n){
                nIndex = i;
            }
        }

        // Special case: if 1 is already at the first position
        // and n is already at the last position, no swaps needed
        if(oneIndex == 0 && nIndex == n - 1){
            return 0;
        }

        // Swap calculation explanation:
        // The formula `oneIndex + (n - 1 - nIndex)` calculates the total number of swaps needed
        // Let's break it down step by step:

        // `oneIndex`:
        // - Represents the number of swaps needed to move 1 to the first position (index 0)
        // - Higher index means more swaps required to bring 1 to the start
        // - Example: In [2,1,4,3], oneIndex is 1, so 1 swap is needed to move 1 to start

        // `(n - 1 - nIndex)`:
        // - Represents the number of swaps needed to move n to the last position (index n-1)
        // - Calculates how far n is from the end of the array
        // - Example: In [2,4,1,3], nIndex is 1, so (3-1-1) = 1 swap is needed to move n to end

        // Total swaps = swaps to move 1 to start + swaps to move n to end
        int swap = oneIndex + (n - 1 - nIndex);

        // Special handling: If 1 appears after n in the original array
        // We can optimize by reducing one swap
        // This is because 1 and n might be swapped in a single operation
        if(oneIndex > nIndex){
            swap--;
        }

        return swap;
    }

    public static void main(String[] args) {
        // Test case 1: Requires 2 swaps
        int[] nums1 = {2, 1, 4, 3};
        System.out.println("Test Case 1: " + semiOrderedPermutation(nums1));

        // Test case 2: Requires 3 swaps
        int[] nums2 = {2, 4, 1, 3};
        System.out.println("Test Case 2: " + semiOrderedPermutation(nums2));

        // Test case 3: Already semi-ordered, no swaps needed
        int[] nums3 = {1, 3, 4, 2, 5};
        System.out.println("Test Case 3: " + semiOrderedPermutation(nums3));

        // Test case 4: 1 at the end, n at the beginning
        int[] nums4 = {5, 2, 3, 4, 1};
        System.out.println("Test Case 4: " + semiOrderedPermutation(nums4));

        // Test case 5: Short array
        int[] nums5 = {2, 1};
        System.out.println("Test Case 5: " + semiOrderedPermutation(nums5));

        // Test case 6: Larger array
        int[] nums6 = {3, 4, 5, 1, 2, 6};
        System.out.println("Test Case 6: " + semiOrderedPermutation(nums6));
    }
}

```





