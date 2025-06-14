---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3542. Minimum Operations to Convert All Elements to Zero"
date: "2025-06-14"
tags: Medium
categories:
  - "LeetCode Stack"
---

- You are given an array `nums` of size `n`, consisting of **non-negative** integers. Your task is to apply some (possibly zero) operations on the array so that **all** elements become 0.
- In one operation, you can select a subarray `[i, j]` (where `0 <= i <= j < n`) and set all occurrences of the **minimum** **non-negative** integer in that subarray to 0.
- Return the **minimum** number of operations required to make all elements in the array 0.

**Example 1**

```
Input: nums = [0,2]

Output: 1

Explanation:

Select the subarray [1,1] (which is [2]), where the minimum non-negative integer is 2. Setting all occurrences of 2 to 0 results in [0,0].
Thus, the minimum number of operations required is 1.
```

**Example 2**

```
Input: nums = [3,1,2,1]

Output: 3

Explanation:

Select subarray [1,3] (which is [1,2,1]), where the minimum non-negative integer is 1. Setting all occurrences of 1 to 0 results in [3,0,2,0].
Select subarray [2,2] (which is [2]), where the minimum non-negative integer is 2. Setting all occurrences of 2 to 0 results in [3,0,0,0].
Select subarray [0,0] (which is [3]), where the minimum non-negative integer is 3. Setting all occurrences of 3 to 0 results in [0,0,0,0].
Thus, the minimum number of operations required is 3.
```

**Example 3**

```
Input: nums = [1,2,1,2,1,2]

Output: 4

Explanation:

Select subarray [0,5] (which is [1,2,1,2,1,2]), where the minimum non-negative integer is 1. Setting all occurrences of 1 to 0 results in [0,2,0,2,0,2].
Select subarray [1,1] (which is [2]), where the minimum non-negative integer is 2. Setting all occurrences of 2 to 0 results in [0,0,0,2,0,2].
Select subarray [3,3] (which is [2]), where the minimum non-negative integer is 2. Setting all occurrences of 2 to 0 results in [0,0,0,0,0,2].
Select subarray [5,5] (which is [2]), where the minimum non-negative integer is 2. Setting all occurrences of 2 to 0 results in [0,0,0,0,0,0].
Thus, the minimum number of operations required is 4.

```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Stack;

import java.util.ArrayDeque;
import java.util.Deque;

/**
 * @Author zhengxingxing
 * @Date 2025/06/14
 */
public class MinimumOperationsToConvertAllElementsToZero {

    public static int minOperations(int[] nums) {
        int ans = 0; // This variable counts the minimum number of operations needed.
        Deque<Integer> stack = new ArrayDeque<>();
        stack.push(0); // Sentinel value to simplify boundary conditions.

        // Iterate through each number in the input array.
        for (int num : nums) {
            // If the current number is less than the top of the stack,
            // pop elements from the stack until the top is less than or equal to the current number.
            // This ensures the stack is always non-decreasing.
            while (!stack.isEmpty() && stack.peek() > num) {
                stack.pop();
            }
            // If the current number is greater than the top of the stack,
            // it means we've found a new "increasing segment" (a new value that hasn't been zeroed yet).
            // We need one more operation for this new segment.
            if (stack.isEmpty() || stack.peek() < num) {
                ans++;         // Increment the operation count.
                stack.push(num); // Push the current number onto the stack as a new segment.
            }
            // If the current number is equal to the top of the stack,
            // it means this value has already been considered, so we do nothing.
        }
        // After processing all elements, 'ans' holds the minimum number of operations needed.
        return ans;
    }

    public static void main(String[] args) {
        // Test case 1: Simple case with a zero and a non-zero value.
        int[] nums1 = {0, 2};
        System.out.println("Input: [0,2]");
        System.out.println("Output: " + minOperations(nums1)); // Expected: 1

        // Test case 2: Array with multiple values and repeated minimums.
        int[] nums2 = {3, 1, 2, 1};
        System.out.println("Input: [3,1,2,1]");
        System.out.println("Output: " + minOperations(nums2)); // Expected: 3

        // Test case 3: Alternating values, requiring multiple operations.
        int[] nums3 = {1, 2, 1, 2, 1, 2};
        System.out.println("Input: [1,2,1,2,1,2]");
        System.out.println("Output: " + minOperations(nums3)); // Expected: 4

        // Edge case: All elements are already zero.
        int[] nums4 = {0, 0, 0};
        System.out.println("Input: [0,0,0]");
        System.out.println("Output: " + minOperations(nums4)); // Expected: 0

        // Edge case: Strictly increasing array.
        int[] nums5 = {1, 2, 3, 4};
        System.out.println("Input: [1,2,3,4]");
        System.out.println("Output: " + minOperations(nums5)); // Expected: 4
    }
}
```





