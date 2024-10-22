---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "453.Minimum Moves to Equal Array Elements"
date: "2024-10-22"
categories:
  - "LeetCode Array"
---




- Given an integer array `nums` of size `n`, return *the minimum number of moves required to make all array elements equal*.
- In one move, you can increment `n - 1` elements of the array by `1`.


**Example 1**

```
Input: nums = [1,2,3]
Output: 3
Explanation: Only three moves are needed (remember each move increments two elements):
[1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]
```

**Example 2**

```
Input: nums = [1,1,1]
Output: 0
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Array;

/**
 * @author zhengstars
 * @date 2024/10/22
 */
public class MinimumMovesToEqualArrayElements {
    public static int minMoves(int[] nums) {
        // Handle edge cases: null array or array with length <= 1
        if (nums == null || nums.length <= 1) {
            return 0;
        }

        // Step 1: Find the minimum value in the array
        // This is our target value that all other elements will eventually equal to
        int min = nums[0];
        for (int num : nums) {
            min = Math.min(min, num);
        }

        // Step 2: Calculate the total moves needed
        // For each element, we need (current value - minimum value) moves to reach the minimum
        int moves = 0;
        for (int num : nums) {
            moves += num - min;  // Add the difference between current element and minimum
        }

        return moves;
    }

    public static void main(String[] args) {

        // Test Case 
        System.out.println("Test Case 1:  " + minMoves(new int[]{ 1,2,3 } ) );  // Expected output: 3
        System.out.println("Test Case 2:  " + minMoves(new int[]{ 1,1,1 } ) );  // Expected output: 0
        System.out.println("Test Case 3:  " + minMoves(new int[]{ 5,8,10 } ) );  // Expected output: 8
        System.out.println("Test Case 4:  " + minMoves(new int[]{ 1,2,2,2,3 } ) );  // Expected output: 6
    }
}

```





