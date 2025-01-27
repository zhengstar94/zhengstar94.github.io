---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "45. Jump Game II"
date: "2025-01-27"
tags: Medium
categories:
  - "LeetCode Greedy"
---


- You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.
- Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:
  - `0 <= j <= nums[i]` and
  - `i + j < n`
- Return *the minimum number of jumps to reach* `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

**Example 1**

```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2**

```
Input: nums = [2,3,0,1,4]
Output: 2
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Greedy;

/**
 * @author zhengxingxing
 * @date 2025/01/27
 */
public class JumpGameII {
    
    public static int jump(int[] nums) {
        // Counter for the minimum number of jumps needed
        int jumps = 0;
        // The farthest position that can be reached in the current jump
        int curEnd = 0;
        // The farthest position that can be reached considering all positions up to current position
        int curFarthest = 0;

        // Iterate through the array (except the last element as we don't need to jump from there)
        for (int i = 0; i < nums.length - 1; i++) {
            // Update the farthest position that can be reached from current position
            curFarthest = Math.max(curFarthest, i + nums[i]);

            // If we've reached the end of current jump range
            if (i == curEnd) {
                // We must take a jump
                jumps++;
                // Update the end range for the next jump
                curEnd = curFarthest;
            }
        }

        return jumps;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Basic case with multiple possible paths
        int[] nums1 = {2,3,1,1,4};
        System.out.println("Test Case 1 Result: " + jump(nums1)); // Expected output: 2

        // Test Case 2: Another case with same minimum jumps but different path
        int[] nums2 = {2,3,0,1,4};
        System.out.println("Test Case 2 Result: " + jump(nums2)); // Expected output: 2

        // Test Case 3: Case where each step can only jump one position
        int[] nums3 = {1,1,1,1};
        System.out.println("Test Case 3 Result: " + jump(nums3)); // Expected output: 3
    }
}

```





