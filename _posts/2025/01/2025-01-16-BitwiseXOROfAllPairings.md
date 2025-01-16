---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2425. Bitwise XOR of All Pairings"
date: "2025-01-16"
tags: Medium
categories:
  - "LeetCode Math"
---


- You are given two **0-indexed** arrays, `nums1` and `nums2`, consisting of non-negative integers. There exists another array, `nums3`, which contains the bitwise XOR of **all pairings** of integers between `nums1` and `nums2` (every integer in `nums1` is paired with every integer in `nums2` **exactly once**).
- Return *the **bitwise XOR** of all integers in* `nums3`.

**Example 1**

```
Input: nums1 = [2,1,3], nums2 = [10,2,5,0]
Output: 13
Explanation:
A possible nums3 array is [8,0,7,2,11,3,4,1,9,1,6,3].
The bitwise XOR of all these numbers is 13, so we return 13.
```

**Example 2**

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 0
Explanation:
All possible pairs of bitwise XORs are nums1[0] ^ nums2[0], nums1[0] ^ nums2[1], nums1[1] ^ nums2[0],
and nums1[1] ^ nums2[1].
Thus, one possible nums3 array is [2,5,1,6].
2 ^ 5 ^ 1 ^ 6 = 0, so we return 0.
```

## Method 1

```tex
【O(n + m) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/01/16
 */
public class BitwiseXOROfAllPairings {
  public static int xorAllNums(int[] nums1, int[] nums2) {
    // Initialize result variable to store the final XOR value
    int result = 0;

    /*
     * First Check: nums2.length % 2 == 1
     * Why this check?
     * - Each number in nums1 will appear nums2.length times in the final calculation
     * - If nums2.length is odd, each number in nums1 will appear odd times
     * - When a number appears odd times in XOR operations, it needs to be XORed once
     * - When a number appears even times, it cancels out (equals 0 in XOR)
     *
     * Example: If nums2.length is 3
     * - Each number in nums1 will appear 3 times in the pairs
     * - 3 times XOR is equivalent to XOR once
     * - Therefore, we need to XOR each number in nums1 once
     */
    if (nums2.length % 2 == 1) {
      for (int num : nums1) {
        result ^= num;  // XOR each number from nums1 once
      }
    }

    /*
     * Second Check: nums1.length % 2 == 1
     * Why this check?
     * - Each number in nums2 will appear nums1.length times in the final calculation
     * - If nums1.length is odd, each number in nums2 will appear odd times
     * - When a number appears odd times in XOR operations, it needs to be XORed once
     * - When a number appears even times, it cancels out (equals 0 in XOR)
     *
     * Example: If nums1.length is 3
     * - Each number in nums2 will appear 3 times in the pairs
     * - 3 times XOR is equivalent to XOR once
     * - Therefore, we need to XOR each number in nums2 once
     */
    if (nums1.length % 2 == 1) {
      for (int num : nums2) {
        result ^= num;  // XOR each number from nums2 once
      }
    }

    // Return the final XOR result
    return result;
  }

  /**
   * Main method to test the solution with various test cases
   */
  public static void main(String[] args) {
    // Test Case 1: nums1 has odd length, nums2 has even length
    int[] nums1_1 = {2, 1, 3};
    int[] nums2_1 = {10, 2, 5, 0};
    System.out.println("Test Case 1 Result: " + xorAllNums(nums1_1, nums2_1)); // Expected: 13

    // Test Case 2: Both arrays have even length
    int[] nums1_2 = {1, 2};
    int[] nums2_2 = {3, 4};
    System.out.println("Test Case 2 Result: " + xorAllNums(nums1_2, nums2_2)); // Expected: 0

    // Test Case 3: Both arrays have odd length
    int[] nums1_3 = {1};
    int[] nums2_3 = {2};
    System.out.println("Test Case 3 Result: " + xorAllNums(nums1_3, nums2_3)); // Expected: 3
  }
}

```





