---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2683. Neighboring Bitwise XOR"
date: "2025-01-17"
tags: Medium
categories:
  - "LeetCode Math"
---



- A **0-indexed** array `derived` with length `n` is derived by computing the **bitwise XOR** (⊕) of adjacent values in a **binary array** `original` of length `n`.
- Specifically, for each index `i` in the range `[0, n - 1]`:
  - If `i = n - 1`, then `derived[i] = original[i] ⊕ original[0]`.
  - Otherwise, `derived[i] = original[i] ⊕ original[i + 1]`.
- Given an array `derived`, your task is to determine whether there exists a **valid binary array** `original` that could have formed `derived`.
- Return ***true** if such an array exists or **false** otherwise.*
  - A binary array is an array containing only **0's** and **1's**

**Example 1**

```
Input: derived = [1,1,0]
Output: true
Explanation: A valid original array that gives derived is [0,1,0].
derived[0] = original[0] ⊕ original[1] = 0 ⊕ 1 = 1 
derived[1] = original[1] ⊕ original[2] = 1 ⊕ 0 = 1
derived[2] = original[2] ⊕ original[0] = 0 ⊕ 0 = 0
```

**Example 2**

```
Input: derived = [1,1]
Output: true
Explanation: A valid original array that gives derived is [0,1].
derived[0] = original[0] ⊕ original[1] = 1
derived[1] = original[1] ⊕ original[0] = 1
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Math;

/**
 * @author zhengxingxing
 * @date 2025/01/17
 */
public class NeighboringBitwiseXOR {
    public static boolean doesValidArrayExist(int[] derived) {
        // Initialize the running XOR result, assuming original[0] = 0
        // This variable tracks the cumulative XOR of all elements
        int original = 0;

        // Iterate through each element in derived array
        // For each iteration, we're essentially checking if the running XOR
        // maintains the necessary relationship between adjacent elements
        for (int num : derived) {
            // XOR current element with running result
            // This operation simulates building the original array and checking
            // if adjacent elements can satisfy the derived array's requirements
            original ^= num;
        }

        // Final check: if original is 0, it means:
        // 1. All elements in the hypothetical original array properly cancel out
        // 2. The circular property (last element XOR first element) is satisfied
        // 3. A valid original array exists
        return original == 0;
    }
    
    public static void main(String[] args) {
        // Test Case 1: [1,1,0] - Valid case
        // Can be derived from original array [0,1,0]
        int[] derived1 = {1, 1, 0};
        System.out.println("Test Case 1 Result: " + doesValidArrayExist(derived1)); // Expected: true

        // Test Case 2: [1,1] - Valid case
        // Can be derived from original array [0,1]
        int[] derived2 = {1, 1};
        System.out.println("Test Case 2 Result: " + doesValidArrayExist(derived2)); // Expected: true

        // Test Case 3: [1,0] - Invalid case
        // No possible original array can generate this derived array
        int[] derived3 = {1, 0};
        System.out.println("Test Case 3 Result: " + doesValidArrayExist(derived3)); // Expected: false

        // Test Case 4: [0] - Valid case
        // Simplest possible case, can be derived from original array [0]
        int[] derived4 = {0};
        System.out.println("Test Case 4 Result: " + doesValidArrayExist(derived4)); // Expected: true
    }
}

```





