---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1052. Grumpy Bookstore Owner"
date: "2024-12-24"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---


- There is a bookstore owner that has a store open for `n` minutes. You are given an integer array `customers` of length `n` where `customers[i]` is the number of the customers that enter the store at the start of the `ith` minute and all those customers leave after the end of that minute.
- During certain minutes, the bookstore owner is grumpy. You are given a binary array grumpy where `grumpy[i]` is `1` if the bookstore owner is grumpy during the `ith` minute, and is `0` otherwise.
- When the bookstore owner is grumpy, the customers entering during that minute are not **satisfied**. Otherwise, they are satisfied.
- The bookstore owner knows a secret technique to remain **not grumpy** for `minutes` consecutive minutes, but this technique can only be used **once**.
- Return the **maximum** number of customers that can be *satisfied* throughout the day.

**Example 1**

```
Input: customers = [1,0,1,2,1,1,7,5], grumpy = [0,1,0,1,0,1,0,1], minutes = 3

Output: 16

Explanation:

The bookstore owner keeps themselves not grumpy for the last 3 minutes.

The maximum number of customers that can be satisfied = 1 + 1 + 1 + 1 + 7 + 5 = 16.
```

**Example 2**

```
Input: customers = [1], grumpy = [0], minutes = 1

Output: 1
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2024/12/24
 */
public class GrumpyBookstoreOwner {
    public static int maxSatisfied(int[] customers, int[] grumpy, int minutes) {
        int n = customers.length;
        if(n == 0){
            return 0;
        }

        // Calculate base satisfied customers (when owner is not grumpy)
        int base = 0;
        for (int i = 0; i < n; i++) {
            if(grumpy[i] == 0){
                base += customers[i];
            }
        }

        // Initialize sliding window sum and maximum increase
        int windowSum = 0;
        int maxIncrease = 0;

        // Process array using sliding window technique
        for (int i = 0; i < n; i++) {
            // Add current customer to window if owner is grumpy
            if (grumpy[i] == 1){
                windowSum += customers[i];
            }

            // Skip until window is fully formed
            if (i < minutes - 1) {
                continue;
            }

            // Update maximum increase for current window
            maxIncrease = Math.max(maxIncrease, windowSum);

            // Remove leftmost element from window
            if (grumpy[i - minutes + 1] == 1) {
                windowSum -= customers[i - minutes + 1];
            }
        }

        // Return total satisfied customers
        return base + maxIncrease;
    }

    public static void main(String[] args) {
        // Test Case 1: Example from problem description
        int[] customers1 = {1, 0, 1, 2, 1, 1, 7, 5};
        int[] grumpy1 = {0, 1, 0, 1, 0, 1, 0, 1};
        int minutes1 = 3;
        System.out.println("Test Case 1:");
        System.out.println("Input: customers = " + arrayToString(customers1) +
                ", grumpy = " + arrayToString(grumpy1) +
                ", minutes = " + minutes1);
        System.out.println("Expected Output: 16");
        System.out.println("Actual Output: " + maxSatisfied(customers1, grumpy1, minutes1));
        System.out.println();

        // Test Case 2: Single element array
        int[] customers2 = {1};
        int[] grumpy2 = {0};
        int minutes2 = 1;
        System.out.println("Test Case 2:");
        System.out.println("Input: customers = " + arrayToString(customers2) +
                ", grumpy = " + arrayToString(grumpy2) +
                ", minutes = " + minutes2);
        System.out.println("Expected Output: 1");
        System.out.println("Actual Output: " + maxSatisfied(customers2, grumpy2, minutes2));
        System.out.println();

        // Test Case 3: All grumpy moments
        int[] customers3 = {1, 2, 3, 4, 5};
        int[] grumpy3 = {1, 1, 1, 1, 1};
        int minutes3 = 3;
        System.out.println("Test Case 3:");
        System.out.println("Input: customers = " + arrayToString(customers3) +
                ", grumpy = " + arrayToString(grumpy3) +
                ", minutes = " + minutes3);
        System.out.println("Expected Output: 9");
        System.out.println("Actual Output: " + maxSatisfied(customers3, grumpy3, minutes3));
        System.out.println();

        // Test Case 4: All non-grumpy moments
        int[] customers4 = {1, 2, 3, 4, 5};
        int[] grumpy4 = {0, 0, 0, 0, 0};
        int minutes4 = 2;
        System.out.println("Test Case 4:");
        System.out.println("Input: customers = " + arrayToString(customers4) +
                ", grumpy = " + arrayToString(grumpy4) +
                ", minutes = " + minutes4);
        System.out.println("Expected Output: 15");
        System.out.println("Actual Output: " + maxSatisfied(customers4, grumpy4, minutes4));
        System.out.println();
    }
    
    private static String arrayToString(int[] arr) {
        StringBuilder sb = new StringBuilder("[");
        for (int i = 0; i < arr.length; i++) {
            sb.append(arr[i]);
            if (i < arr.length - 1) {
                sb.append(",");
            }
        }
        sb.append("]");
        return sb.toString();
    }
}

```





