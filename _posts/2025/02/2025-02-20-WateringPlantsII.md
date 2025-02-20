---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2105. Watering Plants II"
date: "2025-02-20"
tags: Medium
categories:
  - "LeetCode TwoPointers"
---

- Alice and Bob want to water `n` plants in their garden. The plants are arranged in a row and are labeled from `0` to `n - 1` from left to right where the `ith` plant is located at `x = i`.
- Each plant needs a specific amount of water. Alice and Bob have a watering can each, **initially full**. They water the plants in the following way:
  - Alice waters the plants in order from **left to right**, starting from the `0th` plant. Bob waters the plants in order from **right to left**, starting from the `(n - 1)th` plant. They begin watering the plants **simultaneously**.
  - It takes the same amount of time to water each plant regardless of how much water it needs.
  - Alice/Bob **must** water the plant if they have enough in their can to **fully** water it. Otherwise, they **first** refill their can (instantaneously) then water the plant.
  - In case both Alice and Bob reach the same plant, the one with **more** water currently in his/her watering can should water this plant. If they have the same amount of water, then Alice should water this plant.
- Given a **0-indexed** integer array `plants` of `n` integers, where `plants[i]` is the amount of water the `ith` plant needs, and two integers `capacityA` and `capacityB` representing the capacities of Alice's and Bob's watering cans respectively, return *the **number of times** they have to refill to water all the plants*.

**Example 1**

```
Input: plants = [2,2,3,3], capacityA = 5, capacityB = 5
Output: 1
Explanation:
- Initially, Alice and Bob have 5 units of water each in their watering cans.
- Alice waters plant 0, Bob waters plant 3.
- Alice and Bob now have 3 units and 2 units of water respectively.
- Alice has enough water for plant 1, so she waters it. Bob does not have enough water for plant 2, so he refills his can then waters it.
So, the total number of times they have to refill to water all the plants is 0 + 0 + 1 + 0 = 1.
```

**Example 2**

```
Input: plants = [2,2,3,3], capacityA = 3, capacityB = 4
Output: 2
Explanation:
- Initially, Alice and Bob have 3 units and 4 units of water in their watering cans respectively.
- Alice waters plant 0, Bob waters plant 3.
- Alice and Bob now have 1 unit of water each, and need to water plants 1 and 2 respectively.
- Since neither of them have enough water for their current plants, they refill their cans and then water the plants.
So, the total number of times they have to refill to water all the plants is 0 + 1 + 1 + 0 = 2.
```

**Example 3**

```
Input: plants = [5], capacityA = 10, capacityB = 8
Output: 0
Explanation:
- There is only one plant.
- Alice's watering can has 10 units of water, whereas Bob's can has 8 units. Since Alice has more water in her can, she waters this plant.
So, the total number of times they have to refill is 0.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

/**
 * @author zhengxingxing
 * @date 2025/02/20
 */
public class WateringPlantsII {

    public static int minimumRefill(int[] plants, int capacityA, int capacityB) {
        int ans = 0;  // Counter for total refills needed
        int i = 0;    // Left pointer for Alice
        int j = plants.length - 1;  // Right pointer for Bob

        int canA = capacityA;  // Current water amount in Alice's can
        int canB = capacityB;  // Current water amount in Bob's can

        // Process while Alice and Bob haven't met
        while (i < j) {
            // Check if Alice needs a refill
            if (canA < plants[i]) {
                canA = capacityA;  // Refill Alice's can
                ans++;  // Increment refill counter
            }
            canA -= plants[i++];  // Water the plant and move Alice forward

            // Check if Bob needs a refill
            if (canB < plants[j]) {
                canB = capacityB;  // Refill Bob's can
                ans++;  // Increment refill counter
            }
            canB -= plants[j--];  // Water the plant and move Bob backward
        }

        // Handle middle plant (when array length is odd)
        if (i == j) {
            int maxWater = Math.max(canA, canB);  // Use can with more water
            if (maxWater < plants[i]) {
                ans++;  // Need refill if neither can has enough water
            }
        }

        return ans;
    }

    public static void main(String[] args) {
        // Test Case 1: Equal capacity cans
        int[] plants1 = {2, 2, 3, 3};
        int capacityA1 = 5;
        int capacityB1 = 5;
        System.out.println("Test Case 1 Result: " + minimumRefill(plants1, capacityA1, capacityB1)); // Expected: 1

        // Test Case 2: Different capacity cans
        int[] plants2 = {2, 2, 3, 3};
        int capacityA2 = 3;
        int capacityB2 = 4;
        System.out.println("Test Case 2 Result: " + minimumRefill(plants2, capacityA2, capacityB2)); // Expected: 2

        // Test Case 3: Single plant case
        int[] plants3 = {5};
        int capacityA3 = 10;
        int capacityB3 = 8;
        System.out.println("Test Case 3 Result: " + minimumRefill(plants3, capacityA3, capacityB3)); // Expected: 0
    }
}

```





