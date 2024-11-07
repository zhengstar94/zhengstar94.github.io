---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1762.Buildings With an Ocean View LevelI"
date: "2024-11-07"
categories:
  - "LeetCode Stack"
---


- There are `n` buildings in a line. You are given an integer array `heights` of size `n` that represents the heights of the buildings in the line.
- The ocean is to the right of the buildings. A building has an ocean view if the building can see the ocean without obstructions. Formally, a building has an ocean view if all the buildings to its right have a **smaller** height.
- Return a list of indices **(0-indexed)** of buildings that have an ocean view, sorted in increasing order.

**Example 1**

```
Input: heights = [4,2,3,1]
Output: [0,2,3]
Explanation: Building 1 (0-indexed) does not have an ocean view because building 2 is taller.
```

**Example 2**

```
Input: heights = [4,3,2,1]
Output: [0,1,2,3]
Explanation: All the buildings have an ocean view.
```

**Example 3**

```
Input: heights = [1,3,2,4]
Output: [3]
Explanation: Only building 3 has an ocean view.
```

## Method 1

```tex
【O(n) time | O(h) space】
```

```java
package Leetcode.Stack;

import java.util.Stack;

/**
 * @author zhengstars
 * @date 2024/11/07
 */
public class BuildingsWithAnOceanViewLevel {
    public static int[] findBuildings(int[] heights) {
        // Stack to store indices of buildings that can see the ocean
        Stack<Integer> stack = new Stack<>();

        // Iterate from right to left (from ocean side)
        for (int i = heights.length - 1; i >= 0; i--) {
            // A building can see the ocean if either:
            // 1. It's the rightmost building (stack is empty), or
            // 2. It's taller than the tallest building to its right (building at stack.peek())
            if (stack.empty() || heights[i] > heights[stack.peek()]) {
                // If current building is taller, it can see over any building to its right
                stack.push(i);  // Store the index of the building
            }
            // If current building is shorter than or equal to the rightmost visible building,
            // it can't see the ocean, so we skip it
        }

        // Convert stack to array (stack contains indices in reverse order)
        int[] result = new int[stack.size()];
        int index = 0;
        // Pop from stack to get indices in ascending order
        while (!stack.isEmpty()) {
            result[index++] = stack.pop();
        }

        return result;
    }

    public static void main(String[] args) {
        // Test Case 1: Multiple buildings can see the ocean
        int[] heights1 = {4,2,3,1};
        System.out.print("Test Case 1 [4,2,3,1] Output: ");
        printArray(findBuildings(heights1));
        // Expected output: [0,2,3]

        // Test Case 2: All buildings can see the ocean (decreasing heights)
        int[] heights2 = {4,3,2,1};
        System.out.print("Test Case 2 [4,3,2,1] Output: ");
        printArray(findBuildings(heights2));
        // Expected output: [0,1,2,3]

        // Test Case 3: Only the tallest building can see the ocean
        int[] heights3 = {1,3,2,4};
        System.out.print("Test Case 3 [1,3,2,4] Output: ");
        printArray(findBuildings(heights3));
        // Expected output: [3]

        // Test Case 4: All buildings are the same height
        int[] heights4 = {2,2,2,2};
        System.out.print("Test Case 4 [2,2,2,2] Output: ");
        printArray(findBuildings(heights4));
        // Expected output: [3]

        // Test Case 5: Edge case - single building
        int[] heights5 = {5};
        System.out.print("Test Case 5 [5] Output: ");
        printArray(findBuildings(heights5));
        // Expected output: [0]
    }

    private static void printArray(int[] arr) {
        if (arr == null || arr.length == 0) {
            System.out.println("[]");
            return;
        }
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) {
                System.out.print(",");
            }
        }
        System.out.println("]");
    }
}
```





