---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "LCP 18. Breakfast Combination"
date: "2025-03-08"
tags: Easy TwoPointers
categories:
  - "LeetCode DoubleSeqTwoPointers" 
---

- At an autumn market fair, Xiaokei chooses a breakfast stall. A one-dimensional integer array `staple` records the price of each main food, and a one-dimensional integer array `drinks` records the price of each beverage. Xiaokei plans to choose one main food and one drink, with a total cost not exceeding `x` yuan. Please return how many different purchasing combinations Xiaokei has.
- Note: The answer needs to be modulo 1e9 + 7 (1000000007). For example, if the initial result is 1000000008, please return 1.

**Example 1**

```
Input: staple = [10,20,5], drinks = [5,5,2], x = 15
Output: 6
Explanation: Xiaokei has 6 different purchasing combinations, with the corresponding indices of selected main food and drink in the arrays being:
1st combination: staple[0] + drinks[0] = 10 + 5 = 15
2nd combination: staple[0] + drinks[1] = 10 + 5 = 15
3rd combination: staple[0] + drinks[2] = 10 + 2 = 12
4th combination: staple[2] + drinks[0] = 5 + 5 = 10
5th combination: staple[2] + drinks[1] = 5 + 5 = 10
6th combination: staple[2] + drinks[2] = 5 + 2 = 7
```

**Example 2**

```
Input: staple = [2,1,1], drinks = [8,9,5,1], x = 9
Output: 8
Explanation: Xiaokei has 8 different purchasing combinations, with the corresponding indices of selected main food and drink in the arrays being:
1st combination: staple[0] + drinks[2] = 2 + 5 = 7
2nd combination: staple[0] + drinks[3] = 2 + 1 = 3
3rd combination: staple[1] + drinks[0] = 1 + 8 = 9
4th combination: staple[1] + drinks[2] = 1 + 5 = 6
5th combination: staple[1] + drinks[3] = 1 + 1 = 2
6th combination: staple[2] + drinks[0] = 1 + 8 = 9
7th combination: staple[2] + drinks[2] = 1 + 5 = 6
8th combination: staple[2] + drinks[3] = 1 + 1 = 2
```

## Method 1

```tex
【O(nlogn + mlogm) time | O(1) space】
```

```java
package Leetcode.TwoPointer.DoubleSeqTwoPointers;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/03/08
 */
public class BreakfastCombination {

    public static int breakfastNumber(int[] staple, int[] drinks, int x) {
        // Sort both arrays to enable two pointer approach
        Arrays.sort(staple);
        Arrays.sort(drinks);

        // Initialize result as long to prevent overflow during calculations
        long result = 0;
        int mod = 1000000007;

        // Initialize two pointers
        int i = 0;                    // Pointer for staple array starting from left
        int j = drinks.length - 1;    // Pointer for drinks array starting from right

        // Traverse arrays using two pointers
        while (i < staple.length && j >= 0) {
            // If current combination exceeds budget, move drinks pointer left
            // to try a cheaper drink
            if (staple[i] + drinks[j] > x) {
                j--;
            }
            // If combination is within budget
            else {
                // Since arrays are sorted, current staple can combine with all drinks
                // from index 0 to j (inclusive), so add (j+1) combinations
                result = (result + j + 1) % mod;
                // Move to next staple food
                i++;
            }
        }

        return (int)result;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Regular case with multiple valid combinations
        int[] staple1 = {10, 20, 5};
        int[] drinks1 = {5, 5, 2};
        int x1 = 15;
        System.out.println("Test Case 1:");
        System.out.println("Staple prices: " + Arrays.toString(staple1));
        System.out.println("Drink prices: " + Arrays.toString(drinks1));
        System.out.println("Budget: " + x1);
        System.out.println("Number of combinations: " + breakfastNumber(staple1, drinks1, x1));

        // Test Case 2: Case with repeated values
        int[] staple2 = {2, 1, 1};
        int[] drinks2 = {8, 9, 5, 1};
        int x2 = 9;
        System.out.println("\nTest Case 2:");
        System.out.println("Staple prices: " + Arrays.toString(staple2));
        System.out.println("Drink prices: " + Arrays.toString(drinks2));
        System.out.println("Budget: " + x2);
        System.out.println("Number of combinations: " + breakfastNumber(staple2, drinks2, x2));

        // Test Case 3: Edge case with minimum array size
        int[] staple3 = {1};
        int[] drinks3 = {1};
        int x3 = 2;
        System.out.println("\nTest Case 3 (Edge Case):");
        System.out.println("Staple prices: " + Arrays.toString(staple3));
        System.out.println("Drink prices: " + Arrays.toString(drinks3));
        System.out.println("Budget: " + x3);
        System.out.println("Number of combinations: " + breakfastNumber(staple3, drinks3, x3));
    }
}

```





