---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1673. Find the Most Competitive Subsequence"
date: "2025-06-22"
tags: Medium
categories:
  - "LeetCode Stack"
---


- Given an integer array `nums` and a positive integer `k`, return *the most **competitive** subsequence of* `nums` *of size* `k`.
- An array's subsequence is a resulting sequence obtained by erasing some (possibly zero) elements from the array.
- We define that a subsequence `a` is more **competitive** than a subsequence `b` (of the same length) if in the first position where `a` and `b` differ, subsequence `a` has a number **less** than the corresponding number in `b`. For example, `[1,3,4]` is more competitive than `[1,3,5]` because the first position they differ is at the final number, and `4` is less than `5`.

**Example 1**

```
Input: nums = [3,5,2,6], k = 2
Output: [2,6]
Explanation: Among the set of every possible subsequence: {[3,5], [3,2], [3,6], [5,2], [5,6], [2,6]}, [2,6] is the most competitive.
```

**Example 2**

```
Input: nums = [2,4,3,3,5,4,9,6], k = 4
Output: [2,3,3,4]
```

## Method 1

```tex
【O(n) time | O(k) space】
```

```java
package Leetcode.Stack;

import java.util.Arrays;

/**
 * @Author zhengxingxing
 * @Date 2025/06/22
 */
public class FindTheMostCompetitiveSubsequence {
    
    public static int[] mostCompetitive(int[] nums, int k) {
        int n = nums.length;
        // Use an array to simulate a stack, which will store the result subsequence.
        int[] stack = new int[k];
        // 'top' is the pointer to the next available position in the stack (also represents the current stack size).
        int top = 0;

        // Iterate through each element in the input array.
        for (int i = 0; i < n; i++) {
            // While the stack is not empty,
            // and the current element is smaller than the top element of the stack,
            // and there are enough elements left in nums to fill the stack to size k after popping:
            //    - Pop the stack (i.e., remove the last element from the current subsequence).
            //    - This ensures that the subsequence remains as lexicographically small as possible.
            //    - The condition (n - i) > (k - top) is crucial:
            //      It checks if, after popping, there are still enough elements left to fill the subsequence to length k.
            //      If not, we must keep the current stack elements to avoid ending up with fewer than k elements.
            while (top > 0 && stack[top - 1] > nums[i] && (n - i) > (k - top)) {
                top--; // Pop the top element from the stack.
            }
            // If the stack is not yet full (less than k elements), push the current element onto the stack.
            // This ensures we always build a subsequence of exactly length k.
            if (top < k) {
                stack[top++] = nums[i];
            }
        }
        // At the end, 'stack' contains the most competitive subsequence of length k.
        return stack;
    }

    public static void main(String[] args) {
        // Example 1: Input array [3,5,2,6], k = 2
        // Expected output: [2,6]
        System.out.println(Arrays.toString(mostCompetitive(new int[]{3,5,2,6}, 2))); // [2,6]

        // Example 2: Input array [2,4,3,3,5,4,9,6], k = 4
        // Expected output: [2,3,3,4]
        System.out.println(Arrays.toString(mostCompetitive(new int[]{2,4,3,3,5,4,9,6}, 4))); // [2,3,3,4]
    }
}

```





