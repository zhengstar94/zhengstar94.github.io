---
toc:
beginning: true
giscus_comments: true
layout: post
title: "2260. Minimum Consecutive Cards to Pick Up"
date: "2025-07-26"
tags: Medium
categories:
    - "LeetCode HashTable"
---


- You are given an integer array `cards` where `cards[i]` represents the **value** of the `ith` card. A pair of cards are **matching** if the cards have the **same** value.
- Return *the **minimum** number of **consecutive** cards you have to pick up to have a pair of **matching** cards among the picked cards.* If it is impossible to have matching cards, return `-1`.

**Example 1**

```
Input: cards = [3,4,2,3,4,7]
Output: 4
Explanation: We can pick up the cards [3,4,2,3] which contain a matching pair of cards with value 3. Note that picking up the cards [4,2,3,4] is also optimal.
```

**Example 2**

```
Input: cards = [1,0,5,3]
Output: -1
Explanation: There is no way to pick up a set of consecutive cards that contain a pair of matching cards.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.HashTable;

import java.util.HashMap;

/**
 * @Author zhengxingxing
 * @Date 2025/07/26
 */
public class MinimumConsecutiveCardsToPickUp {

    public static int minimumCardPickup(int[] cards) {
        // HashMap to store the last index where each card value appeared
        HashMap<Integer, Integer> lastIndex = new HashMap<>();
        // Initialize the minimum length to a very large value
        int minLen = Integer.MAX_VALUE;

        // Iterate through the cards array
        for (int i = 0; i < cards.length; i++) {
            // If the current card value has appeared before
            if (lastIndex.containsKey(cards[i])){
                // Get the previous index where this card value appeared
                int prevIndex = lastIndex.get(cards[i]);
                // Calculate the length of the subarray between the two matching cards (inclusive)
                minLen = Math.min(minLen, i - prevIndex + 1);
            }
            // Update the last seen index for the current card value
            lastIndex.put(cards[i], i);
        }
        // If minLen was not updated, return -1 (no matching pair found)
        // Otherwise, return the minimum length found
        return minLen == Integer.MAX_VALUE ? -1 : minLen;
    }

    public static void main(String[] args) {
        // Test case 1: There is a matching pair (3 appears twice)
        int[] cards1 = {3, 4, 2, 3, 4, 7};
        // Test case 2: No matching pairs
        int[] cards2 = {1, 0, 5, 3};
        // Output the results for both test cases
        System.out.println(minimumCardPickup(cards1)); // Output: 4
        System.out.println(minimumCardPickup(cards2)); // Output: -1
    }
}

```





