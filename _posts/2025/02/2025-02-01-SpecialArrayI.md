---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3151. Special Array I"
date: "2025-02-01"
tags: Easy
categories:
  - "LeetCode Array"
---


- An array is considered **special** if every pair of its adjacent elements contains two numbers with different parity.
- You are given an array of integers `nums`. Return `true` if `nums` is a **special** array, otherwise, return `false`.

**Example 1**

```
Input: nums = [1]

Output: true

Explanation:

There is only one element. So the answer is true.
```

**Example 2**

```
Input: nums = [2,1,4]

Output: true

Explanation:

There is only two pairs: (2,1) and (1,4), and both of them contain numbers with different parity. So the answer is true.
```

**Example 3**

```
Input: nums = [4,3,1,6]

Output: false

Explanation:

nums[1] and nums[2] are both odd. So the answer is false.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengxingxing
 * @date 2025/02/01
 */
public class SpecialArrayI {
    public static boolean isArraySpecial(int[] nums) {
        // Iterate through the array up to the second-to-last element
        // We use length-1 because we're checking pairs of elements (current and next)
        for (int i = 0; i < nums.length - 1; i++) {
            // Check if current element and next element have the same parity
            // nums[i] % 2 gets the remainder (0 for even, 1 for odd)
            // If both elements have the same parity (both odd or both even),
            // then the array is not special
            if (nums[i] % 2 == nums[i + 1] % 2) {
                return false;  // Found two adjacent elements with same parity
            }
        }
        // If we've made it through the entire array without finding
        // any adjacent elements with the same parity, the array is special
        return true;
    }

    public static void main(String[] args) {
        // Test Case 1: Single element array
        // Any single element array is considered special by default
        System.out.println("Test case 1: " + isArraySpecial(new int[]{1})); // Expected: true

        // Test Case 2: Array with alternating even-odd parity
        // [2(even), 1(odd), 4(even)] - all adjacent pairs have different parity
        System.out.println("Test case 2: " + isArraySpecial(new int[]{2, 1, 4})); // Expected: true

        // Test Case 3: Array with adjacent odd numbers
        // [4(even), 3(odd), 1(odd), 6(even)] - contains adjacent odd numbers
        System.out.println("Test case 3: " + isArraySpecial(new int[]{4, 3, 1, 6})); // Expected: false

        // Test Case 4: Longer array with alternating parity
        // [2(even), 1(odd), 4(even), 3(odd), 6(even), 5(odd)]
        System.out.println("Test case 4: " + isArraySpecial(new int[]{2, 1, 4, 3, 6, 5})); // Expected: true

        // Test Case 5: Array with adjacent even numbers
        // [2(even), 2(even), 4(even)] - contains adjacent even numbers
        System.out.println("Test case 5: " + isArraySpecial(new int[]{2, 2, 4})); // Expected: false
    }
}

```





