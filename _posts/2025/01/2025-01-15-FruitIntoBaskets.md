---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "904. Fruit Into Baskets"
date: "2025-01-15"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindowMax"
---


- You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the **type** of fruit the `ith` tree produces.
- You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:
  - You only have **two** baskets, and each basket can only hold a **single type** of fruit. There is no limit on the amount of fruit each basket can hold.
  - Starting from any tree of your choice, you must pick **exactly one fruit** from **every** tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
  - Once you reach a tree with fruit that cannot fit in your baskets, you must stop.
- Given the integer array `fruits`, return *the **maximum** number of fruits you can pick*.

**Example 1**

```
Input: fruits = [1,2,1]
Output: 3
Explanation: We can pick from all 3 trees.
```

**Example 2**

```
Input: fruits = [0,1,2,2]
Output: 3
Explanation: We can pick from trees [1,2,2].
If we had started at the first tree, we would only pick from trees [0,1].
```

**Example 3**

```
Input: fruits = [1,2,3,2,2]
Output: 4
Explanation: We can pick from trees [2,3,2,2].
If we had started at the first tree, we would only pick from trees [1,2].
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

import java.util.HashMap;
import java.util.Map;

/**
 * @author zhengxingxing
 * @date 2025/01/15
 */
public class FruitIntoBaskets {
    public static int totalFruit(int[] fruits) {
        int maxFruits = 0; // Maximum number of fruits
        int left = 0;      // Left pointer of sliding window
        Map<Integer, Integer> basket = new HashMap<>(); // Map to track fruit types and their counts

        for (int right = 0; right < fruits.length; right++) {
            // Add current fruit to the basket
            basket.put(fruits[right], basket.getOrDefault(fruits[right], 0) + 1);

            // While we have more than 2 types of fruits in the basket
            while (basket.size() > 2) {
                int leftFruit = fruits[left]; // Get the fruit type at the left pointer
                basket.put(leftFruit, basket.get(leftFruit) - 1); // Decrease the count of that fruit

                // If the count becomes 0, remove this fruit type from the basket
                if (basket.get(leftFruit) == 0) {
                    basket.remove(leftFruit);
                }

                // Move the left pointer to shrink the window
                left++;
            }

            // Update maximum fruits count
            maxFruits = Math.max(maxFruits, right - left + 1);
        }

        return maxFruits; // Return the maximum number of fruits collected
    }

    public static void main(String[] args) {

        // Test Case 1
        int[] test1 = {1, 2, 1};
        System.out.println("Test Case 1 Result: " + totalFruit(test1)); // Expected: 3

        // Test Case 2
        int[] test2 = {0, 1, 2, 2};
        System.out.println("Test Case 2 Result: " + totalFruit(test2)); // Expected: 3

        // Test Case 3
        int[] test3 = {1, 2, 3, 2, 2};
        System.out.println("Test Case 3 Result: " + totalFruit(test3)); // Expected: 4

        // Test Case 4
        int[] test4 = {1, 1, 1, 1};
        System.out.println("Test Case 4 Result: " + totalFruit(test4)); // Expected: 4

        // Test Case 5
        int[] test5 = {};
        System.out.println("Test Case 5 Result: " + totalFruit(test5)); // Expected: 0

        // Test Case 6
        int[] test6 = {5, 2, 5, 2, 5, 2};
        System.out.println("Test Case 6 Result: " + totalFruit(test6)); // Expected: 6
    }
}

```





