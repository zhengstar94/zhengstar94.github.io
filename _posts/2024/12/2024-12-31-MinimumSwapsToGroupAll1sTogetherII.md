---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2134. Minimum Swaps to Group All 1's Together II"
date: "2024-12-31"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---


- A **swap** is defined as taking two **distinct** positions in an array and swapping the values in them.
- A **circular** array is defined as an array where we consider the **first** element and the **last** element to be **adjacent**.
- Given a **binary** **circular** array `nums`, return *the minimum number of swaps required to group all* `1`*'s present in the array together at **any location***.

**Example 1**

```
Input: nums = [0,1,0,1,1,0,0]
Output: 1
Explanation: Here are a few of the ways to group all the 1's together:
[0,0,1,1,1,0,0] using 1 swap.
[0,1,1,1,0,0,0] using 1 swap.
[1,1,0,0,0,0,1] using 2 swaps (using the circular property of the array).
There is no way to group all 1's together with 0 swaps.
Thus, the minimum number of swaps required is 1.
```

**Example 2**

```
Input: nums = [0,1,1,1,0,0,1,1,0]
Output: 2
Explanation: Here are a few of the ways to group all the 1's together:
[1,1,1,0,0,0,0,1,1] using 2 swaps (using the circular property of the array).
[1,1,1,1,1,0,0,0,0] using 2 swaps.
There is no way to group all 1's together with 0 or 1 swaps.
Thus, the minimum number of swaps required is 2.
```

**Example 3**

```
Input: nums = [1,1,0,0,1]
Output: 0
Explanation: All the 1's are already grouped together due to the circular property of the array.
Thus, the minimum number of swaps required is 0.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2024/12/31
 */
public class MinimumSwapsToGroupAll1sTogetherII {
    public static int minSwaps(int[] nums) {
        // Edge case: if array is null or length less than 2, no swaps needed
        if (nums == null || nums.length < 2) {
            return 0;
        }

        // Step 1: Count the number of 1's in the array
        int n = nums.length;
        int onesCount = 0;
        for (int num : nums) {
            if (num == 1) {
                onesCount++;
            }
        }

        // If there are 0 or 1 ones, no swaps needed
        if (onesCount <= 1) {
            return 0;
        }

        // Step 2: Create double-length array to handle circular nature
        // Example: [1,0,1] -> [1,0,1,1,0,1]
        int[] doubleNums = new int[2 * n];
        for (int i = 0; i < n; i++) {
            doubleNums[i] = nums[i];
            doubleNums[i + n] = nums[i];  // Copy first half to second half
        }

        // Step 3: Initialize the first window
        // Count zeros in the first window of size onesCount
        int windowZeros = 0;  // Tracks number of zeros in current window
        for (int i = 0; i < onesCount; i++) {
            if (doubleNums[i] == 0) {
                windowZeros++;
            }
        }

        // Initialize minZeros with first window's zero count
        int minZeros = windowZeros;

        // Step 4: Slide the window and find minimum zeros
        // Example: For array [1,0,1,0,1] with onesCount = 3
        // Windows: [1,0,1], [0,1,0], [1,0,1], [0,1,1], etc.
        for (int i = onesCount; i < n + onesCount; i++) {
            // Remove leftmost element from window
            // If it's 0, decrease window's zero count
            if (doubleNums[i - onesCount] == 0) {
                windowZeros--;
            }

            // Add rightmost element to window
            // If it's 0, increase window's zero count
            if (doubleNums[i] == 0) {
                windowZeros++;
            }

            /* Update minimum zeros if current window has fewer zeros
             * Why this represents minimum swaps:
             * 1. Each window of size onesCount represents a potential position for grouped 1's
             * 2. Number of zeros in window = number of swaps needed for that position
             * Example: Window [1,0,1] has 1 zero
             * - Need 1 swap to make it [1,1,1]
             * Window [0,1,0] has 2 zeros
             * - Need 2 swaps to make it [1,1,1]
             * Therefore, minimum zeros across all windows = minimum swaps needed
             */
            minZeros = Math.min(minZeros, windowZeros);
        }

        // Return the minimum number of zeros found in any window
        // This equals the minimum number of swaps needed
        return minZeros;
    }

    public static void main(String[] args) {
        // Test Case 1: Basic case
        int[] test1 = {1, 0, 1, 0, 1};
        System.out.println("Test Case 1: [1,0,1,0,1]");
        System.out.println("Result: " + minSwaps(test1));
        System.out.println();

        // Test Case 2: All ones
        int[] test2 = {1, 1, 1};
        System.out.println("Test Case 2: [1,1,1]");
        System.out.println("Result: " + minSwaps(test2));
        System.out.println();

        // Test Case 3: All zeros
        int[] test3 = {0, 0, 0};
        System.out.println("Test Case 3: [0,0,0]");
        System.out.println("Result: " + minSwaps(test3));
        System.out.println();

        // Test Case 4: Multiple swaps needed
        int[] test4 = {1, 0, 1, 0, 1, 0};
        System.out.println("Test Case 4: [1,0,1,0,1,0]");
        System.out.println("Result: " + minSwaps(test4));
        System.out.println();

        // Test Case 5: Edge case - empty array
        int[] test5 = {};
        System.out.println("Test Case 5: []");
        System.out.println("Result: " + minSwaps(test5));
    }
}

```





