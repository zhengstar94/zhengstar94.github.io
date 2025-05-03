---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1007. Minimum Domino Rotations For Equal Row"
date: "2025-05-03"
tags: Medium
categories:
  - "LeetCode Greedy"
---


- In a row of dominoes, `tops[i]` and `bottoms[i]` represent the top and bottom halves of the `ith` domino. (A domino is a tile with two numbers from 1 to 6 - one on each half of the tile.)
- We may rotate the `ith` domino, so that `tops[i]` and `bottoms[i]` swap values.
- Return the minimum number of rotations so that all the values in `tops` are the same, or all the values in `bottoms` are the same.
- If it cannot be done, return `-1`.

**Example 1**

```
Input: tops = [2,1,2,4,2,2], bottoms = [5,2,6,2,3,2]
Output: 2
Explanation: 
The first figure represents the dominoes as given by tops and bottoms: before we do any rotations.
If we rotate the second and fourth dominoes, we can make every value in the top row equal to 2, as indicated by the second figure.
```

**Example 2**

```
Input: tops = [3,5,1,2,3], bottoms = [3,6,3,3,4]
Output: -1
Explanation: 
In this case, it is not possible to rotate the dominoes to make one row of values equal.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Greedy;

/**
 * @author zhengxingxing
 * @date 2025/05/03
 */
public class MinimumDominoRotationsForEqualRow {

    public static int minDominoRotations(int[] tops, int[] bottoms) {
        // We only need to check two potential target values: tops[0] and bottoms[0]
        // This is because if a solution exists, the target value must appear in the first domino
        int ans = Math.min(
                minRot(tops, bottoms, tops[0]),     // Try to make all tops or bottoms equal to tops[0]
                minRot(tops, bottoms, bottoms[0])   // Try to make all tops or bottoms equal to bottoms[0]
        );

        // If both attempts returned Integer.MAX_VALUE, it means no solution exists
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }
    private static int minRot(int[] tops, int[] bottoms, int target) {
        int toTop = 0;    // Count of rotations needed to make all tops equal to target
        int toBottom = 0; // Count of rotations needed to make all bottoms equal to target

        // Iterate through each domino
        for (int i = 0; i < tops.length; i++) {
            int x = tops[i];    // Current top value
            int y = bottoms[i]; // Current bottom value

            // If neither the top nor bottom is the target value, it's impossible to achieve our goal
            if (x != target && y != target) {
                return Integer.MAX_VALUE;
            }

            // If top is not the target value, we need one rotation to make tops[i] = target
            if (x != target) {
                toTop++;    // Increment the count for making all tops the target
            }
            // If bottom is not the target value, we need one rotation to make bottoms[i] = target
            else if (y != target) {
                toBottom++; // Increment the count for making all bottoms the target
            }
            // If both are the target value, no rotation is needed for this domino
        }

        // Return the minimum of the two approaches:
        // 1. Make all tops the target value
        // 2. Make all bottoms the target value
        return Math.min(toTop, toBottom);
    }
    
    public static void main(String[] args) {
        // Test Case 1
        int[] tops1 = {2, 1, 2, 4, 2, 2};
        int[] bottoms1 = {5, 2, 6, 2, 3, 2};
        System.out.println("Test Case 1:");
        System.out.println("Input: tops = [2,1,2,4,2,2], bottoms = [5,2,6,2,3,2]");
        System.out.println("Expected Output: 2");
        System.out.println("Actual Output: " + minDominoRotations(tops1, bottoms1));
        System.out.println();

        // Test Case 2
        int[] tops2 = {3, 5, 1, 2, 3};
        int[] bottoms2 = {3, 6, 3, 3, 4};
        System.out.println("Test Case 2:");
        System.out.println("Input: tops = [3,5,1,2,3], bottoms = [3,6,3,3,4]");
        System.out.println("Expected Output: -1");
        System.out.println("Actual Output: " + minDominoRotations(tops2, bottoms2));
        System.out.println();

        // Test Case 3
        int[] tops3 = {1, 2, 1, 1, 1, 2, 2, 2};
        int[] bottoms3 = {2, 1, 2, 2, 2, 2, 2, 2};
        System.out.println("Test Case 3:");
        System.out.println("Input: tops = [1,2,1,1,1,2,2,2], bottoms = [2,1,2,2,2,2,2,2]");
        System.out.println("Expected Output: 1");
        System.out.println("Actual Output: " + minDominoRotations(tops3, bottoms3));
    }
}
```





