---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "948. Bag of Tokens"
date: "2025-02-25"
tags: Medium TwoPointers
categories:
  - "LeetCode SingleSeqTwoPointersInward" 
---


- You start with an initial **power** of `power`, an initial **score** of `0`, and a bag of tokens given as an integer array `tokens`, where each `tokens[i]` denotes the value of token*i*.
- Your goal is to **maximize** the total **score** by strategically playing these tokens. In one move, you can play an **unplayed** token in one of the two ways (but not both for the same token):
  - **Face-up**: If your current power is **at least** `tokens[i]`, you may play token*i*, losing `tokens[i]` power and gaining `1` score.
  - **Face-down**: If your current score is **at least** `1`, you may play token*i*, gaining `tokens[i]` power and losing `1` score.
- Return *the **maximum** possible score you can achieve after playing **any** number of tokens*.


**Example 1**

```
Input: tokens = [100], power = 50

Output: 0

Explanation: Since your score is 0 initially, you cannot play the token face-down. You also cannot play it face-up since your power (50) is less than tokens[0] (100).
```

**Example 2**

```
Input: tokens = [200,100], power = 150

Output: 1

Explanation: Play token1 (100) face-up, reducing your power to 50 and increasing your score to 1.

There is no need to play token0, since you cannot play it face-up to add to your score. The maximum score achievable is 1.
```

**Example 3**

```
Input: tokens = [100,200,300,400], power = 200

Output: 2

Explanation: Play the tokens in this order to get a score of 2:

Play token0 (100) face-up, reducing power to 100 and increasing score to 1.
Play token3 (400) face-down, increasing power to 500 and reducing score to 0.
Play token1 (200) face-up, reducing power to 300 and increasing score to 1.
Play token2 (300) face-up, reducing power to 0 and increasing score to 2.
The maximum score achievable is 2.
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.TwoPointer;

import java.util.Arrays;

/**
 * @author zhengxingxing
 * @date 2025/02/25
 */
public class BagOfTokens {
    
    public static int bagOfTokensScore(int[] tokens, int power) {
        // Handle edge cases: null array or empty array
        if (tokens == null || tokens.length == 0) {
            return 0;
        }

        // Sort tokens in ascending order to optimize token usage
        // Smallest tokens will be used for face-up plays
        // Largest tokens will be used for face-down plays
        Arrays.sort(tokens);

        // Initialize two pointers:
        // left: points to the smallest unused token
        // right: points to the largest unused token
        int left = 0;
        int right = tokens.length - 1;

        // Track both current and maximum scores
        // currentScore: score at current state
        // maxScore: highest score achieved so far
        int currentScore = 0;
        int maxScore = 0;

        /* Main game loop:
         * Continue while there are tokens to play (left <= right) AND
         * either we have enough power to play face-up (power >= tokens[left])
         * or we have score to play face-down (currentScore > 0)
         */
        while (left <= right && (power >= tokens[left] || currentScore > 0)) {
            /* Face-up play loop:
             * While we have enough power, keep playing tokens face-up
             * This maximizes our current score using minimum power
             */
            while (left <= right && power >= tokens[left]) {
                power -= tokens[left];    // Spend power
                currentScore++;           // Gain one score
                left++;                   // Move to next smallest token
            }

            // Update maximum score achieved after face-up plays
            maxScore = Math.max(maxScore, currentScore);

            /* Face-down play:
             * If we can't play face-up but have score to spend,
             * play the largest remaining token face-down to gain more power
             */
            if (left <= right && currentScore > 0) {
                power += tokens[right];   // Gain power
                currentScore--;           // Spend one score
                right--;                  // Move to next largest token
            }
        }

        return maxScore;
    }

    
    public static void main(String[] args) {
        // Test Case 1: Cannot play any token
        // Expected output: 0
        int[] tokens1 = {100};
        int power1 = 50;
        System.out.println("Test Case 1 Result: " + bagOfTokensScore(tokens1, power1));

        // Test Case 2: Can play one token face-up
        // Expected output: 1
        int[] tokens2 = {200, 100};
        int power2 = 150;
        System.out.println("Test Case 2 Result: " + bagOfTokensScore(tokens2, power2));

        // Test Case 3: Complex case with multiple plays
        // Expected output: 2
        int[] tokens3 = {100, 200, 300, 400};
        int power3 = 200;
        System.out.println("Test Case 3 Result: " + bagOfTokensScore(tokens3, power3));
    }
}

```





