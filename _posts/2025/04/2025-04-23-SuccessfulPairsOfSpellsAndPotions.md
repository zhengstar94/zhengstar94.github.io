---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2300. Successful Pairs of Spells and Potions"
date: "2025-04-23"
tags: Medium
categories:
  - "LeetCode BinarySearch"
---


- You are given two positive integer arrays `spells` and `potions`, of length `n` and `m` respectively, where `spells[i]` represents the strength of the `ith` spell and `potions[j]` represents the strength of the `jth` potion.
- You are also given an integer `success`. A spell and potion pair is considered **successful** if the **product** of their strengths is **at least** `success`.
- Return *an integer array* `pairs` *of length* `n` *where* `pairs[i]` *is the number of **potions** that will form a successful pair with the* `ith` *spell.*

**Example 1**

```
Input: spells = [5,1,3], potions = [1,2,3,4,5], success = 7
Output: [4,0,3]
Explanation:
- 0th spell: 5 * [1,2,3,4,5] = [5,10,15,20,25]. 4 pairs are successful.
- 1st spell: 1 * [1,2,3,4,5] = [1,2,3,4,5]. 0 pairs are successful.
- 2nd spell: 3 * [1,2,3,4,5] = [3,6,9,12,15]. 3 pairs are successful.
Thus, [4,0,3] is returned.
```

**Example 2**

```
Input: spells = [3,1,2], potions = [8,5,8], success = 16
Output: [2,0,2]
Explanation:
- 0th spell: 3 * [8,5,8] = [24,15,24]. 2 pairs are successful.
- 1st spell: 1 * [8,5,8] = [8,5,8]. 0 pairs are successful. 
- 2nd spell: 2 * [8,5,8] = [16,10,16]. 2 pairs are successful. 
Thus, [2,0,2] is returned.
```

**Example 3**

```
Input: nums = [5,20,66,1314]
Output: 4
Explanation: There are 4 positive integers and 0 negative integers. The maximum count among them is 4.
```

## Method 1

```tex
【O((n + m) log m) time | O(1) space】
```

```java
package Leetcode.BinarySearch;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/04/23
 */
public class SuccessfulPairsOfSpellsAndPotions {

    public static int[] successfulPairs(int[] spells, int[] potions, long success) {
        // Sort potions array in ascending order to enable binary search
        // Time complexity: O(mlogm)
        Arrays.sort(potions);

        // Initialize variables for array lengths and result array
        int n = spells.length;  // number of spells
        int m = potions.length; // number of potions
        int[] result = new int[n];

        // Process each spell to find successful combinations
        // Time complexity: O(nlogm)
        for (int i = 0; i < n; i++) {
            // Calculate minimum potion value needed for current spell
            // Formula: ceil(success/spell) = (success + spell - 1) / spell
            // This ensures we get ceiling value without using Math.ceil()
            long minPotion = (success + spells[i] - 1) / spells[i];

            // Use binary search to find the first potion that when combined
            // with current spell produces value >= success
            int index = binarySearch(potions, minPotion);

            // Calculate number of successful combinations:
            // Total potions - index of first valid potion
            result[i] = m - index;
        }

        return result;
    }

    private static int binarySearch(int[] arr, long target) {
        // Initialize search boundaries
        int left = 0;                // Start of search range
        int right = arr.length;      // End of search range (exclusive)

        // Continue searching while boundaries haven't crossed
        while (left < right) {
            // Calculate middle point
            // Use (right-left)/2 to avoid integer overflow
            int mid = left + (right - left) / 2;

            // If middle element is less than target
            // Search in right half: [mid+1, right]
            if (arr[mid] < target) {
                left = mid + 1;
            }
            // If middle element is >= target
            // Search in left half including mid: [left, mid]
            else {
                right = mid;
            }
        }

        // Return position where element should be inserted
        // This is either the index of first element >= target
        // or arr.length if no such element exists
        return left;
    }


    public static void main(String[] args) {
        // Test Case 1: Mixed positive numbers with multiple successful combinations
        int[] spells1 = {5,1,3};
        int[] potions1 = {1,2,3,4,5};
        long success1 = 7;
        System.out.println("Test Case 1: " +
                Arrays.toString(successfulPairs(spells1, potions1, success1))); // Expected: [4,0,3]

        // Test Case 2: Larger numbers testing overflow handling
        int[] spells2 = {3,1,2};
        int[] potions2 = {8,5,8};
        long success2 = 16;
        System.out.println("Test Case 2: " +
                Arrays.toString(successfulPairs(spells2, potions2, success2))); // Expected: [2,0,2]

        // Test Case 3: Sequential numbers testing boundary conditions
        int[] spells3 = {1,2,3,4,5};
        int[] potions3 = {1,2,3,4,5};
        long success3 = 10;
        System.out.println("Test Case 3: " +
                Arrays.toString(successfulPairs(spells3, potions3, success3))); // Expected: [0,0,1,2,3]
    }
}

```





