---
toc:
beginning: true
giscus_comments: true
layout: post
title: "2410. Maximum Matching of Players With Trainers"
date: "2025-07-13"
tags: Medium
categories:
- "LeetCode Greedy"
---


- You are given a **0-indexed** integer array `players`, where `players[i]` represents the **ability** of the `ith` player.
- You are also given a **0-indexed** integer array `trainers`, where `trainers[j]` represents the **training capacity** of the `jth` trainer.
- The `ith` player can **match** with the `jth` trainer if the player's ability is **less than or equal to** the trainer's training capacity. Additionally, the `ith` player can be matched with at most one trainer, and the `jth` trainer can be matched with at most one player.
- Return *the **maximum** number of matchings between* `players` *and* `trainers` *that satisfy these conditions.*

**Example 1**

```
Input: players = [4,7,9], trainers = [8,2,5,8]
Output: 2
Explanation:
One of the ways we can form two matchings is as follows:
- players[0] can be matched with trainers[0] since 4 <= 8.
- players[1] can be matched with trainers[3] since 7 <= 8.
It can be proven that 2 is the maximum number of matchings that can be formed.
```

**Example 2**

```
Input: players = [1,1,1], trainers = [10]
Output: 1
Explanation:
The trainer can be matched with any of the 3 players.
Each player can only be matched with one trainer, so the maximum answer is 1.
```

## Method 1

```tex
【O(nlogn + mlogm) time | O(1) space】
```

```java
package Leetcode.Greedy;

import java.util.Arrays;

/**
 * @Author zhengxingxing
 * @Date 2025/07/13
 */
public class MaximumMatchingOfPlayersWithTrainers {

    public static int matchPlayersAndTrainers(int[] players, int[] trainers) {
        // Sort both arrays in ascending order.
        // This allows us to use the greedy strategy: always try to match the weakest available player
        // with the weakest available trainer who can train them.
        Arrays.sort(players);
        Arrays.sort(trainers);

        int i = 0;      // Pointer for players array
        int j = 0;      // Pointer for trainers array
        int count = 0;  // Counter for successful matchings

        // Use two pointers to traverse both arrays
        while (i < players.length && j < trainers.length) {
            // If the current player can be matched with the current trainer
            if (players[i] <= trainers[j]) {
                // Successful match: increment the count
                count++;
                // Move to the next player and the next trainer
                i++;
                j++;
            } else {
                // If the current trainer is too weak for the current player,
                // try the next trainer (since trainers are sorted, no later trainer will be weaker)
                j++;
            }
        }
        // Return the total number of successful matchings
        return count;
    }

    public static void main(String[] args) {
        // Test case 1: Expected output is 2
        int[] players1 = {4, 7, 9};
        int[] trainers1 = {8, 2, 5, 8};
        System.out.println(matchPlayersAndTrainers(players1, trainers1)); // Output: 2

        // Test case 2: Expected output is 1
        int[] players2 = {1, 1, 1};
        int[] trainers2 = {10};
        System.out.println(matchPlayersAndTrainers(players2, trainers2)); // Output: 1
        
    }
}

```





