---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "3477. Fruits Into Baskets II"
date: "2025-08-05"
tags: Easy
categories:
  - "LeetCode Greedy"
---



- You are given two arrays of integers, `fruits` and `baskets`, each of length `n`, where `fruits[i]` represents the **quantity** of the `ith` type of fruit, and `baskets[j]` represents the **capacity** of the `jth` basket.
- From left to right, place the fruits according to these rules:
  - Each fruit type must be placed in the **leftmost available basket** with a capacity **greater than or equal** to the quantity of that fruit type.
  - Each basket can hold **only one** type of fruit.
  - If a fruit type **cannot be placed** in any basket, it remains **unplaced**.
- Return the number of fruit types that remain unplaced after all possible allocations are made.

**Example 1**

```
Input: fruits = [4,2,5], baskets = [3,5,4]
Output: 1
Explanation:
- fruits[0] = 4 is placed in baskets[1] = 5.
- fruits[1] = 2 is placed in baskets[0] = 3.
- fruits[2] = 5 cannot be placed in baskets[2] = 4.
Since one fruit type remains unplaced, we return 1.
```

**Example 2**

```
Input: fruits = [3,6,1], baskets = [6,4,7]
Output: 0
Explanation:
- fruits[0] = 3 is placed in baskets[0] = 6.
- fruits[1] = 6 cannot be placed in baskets[1] = 4 (insufficient capacity) but can be placed in the next available basket, baskets[2] = 7.
- fruits[2] = 1 is placed in baskets[1] = 4.
Since all fruits are successfully placed, we return 0.
```

## Method 1

```tex
【O(n^2) time | O(n) space】
```

```java
package Leetcode.Greedy;

/**
 * @Author zhengxingxing
 * @Date 2025/08/05
 */
public class FruitsIntoBasketsII {

    public static int unplacedFruits(int[] fruits, int[] baskets) {
        int n = fruits.length; // Number of fruit types (and baskets, since arrays are the same length)
        boolean[] used = new boolean[n]; // used[j] indicates whether basket j has already been used
        int unplaced = 0; // Counter for fruit types that could not be placed

        // Iterate over each fruit type
        for (int i = 0; i < n; i++) {
            boolean placed = false; // Flag to check if current fruit type has been placed

            // Try to find the first available basket for the current fruit type
            for (int j = 0; j < n; j++) {
                // Check if basket j is unused and has enough capacity for fruits[i]
                if (!used[j] && baskets[j] >= fruits[i]){
                    used[j] = true; // Mark this basket as used
                    placed = true;  // Mark this fruit type as placed
                    break;          // Stop searching for baskets for this fruit type
                }
            }

            // If no suitable basket was found, increment the unplaced counter
            if (!placed) {
                unplaced++;
            }
        }

        // Return the total number of unplaced fruit types
        return unplaced;
    }

    public static void main(String[] args) {
        int[] fruits1 = {4, 2, 5};
        int[] baskets1 = {3, 5, 4};
        // Expected output: 1
        System.out.println(unplacedFruits(fruits1, baskets1));

        int[] fruits2 = {3, 6, 1};
        int[] baskets2 = {6, 4, 7};
        // Expected output: 0
        System.out.println(unplacedFruits(fruits2, baskets2));
    }
}

```





