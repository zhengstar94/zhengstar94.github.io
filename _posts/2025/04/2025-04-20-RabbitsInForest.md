---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "781. Rabbits in Forest"
date: "2025-04-20"
tags: Medium
categories:
  - "LeetCode Greedy"
---


- There is a forest with an unknown number of rabbits. We asked n rabbits **"How many rabbits have the same color as you?"** and collected the answers in an integer array `answers` where `answers[i]` is the answer of the `ith` rabbit.
- Given the array `answers`, return *the minimum number of rabbits that could be in the forest*.

**Example 1**

```
Input: answers = [1,1,2]
Output: 5
Explanation:
The two rabbits that answered "1" could both be the same color, say red.
The rabbit that answered "2" can't be red or the answers would be inconsistent.
Say the rabbit that answered "2" was blue.
Then there should be 2 other blue rabbits in the forest that didn't answer into the array.
The smallest possible number of rabbits in the forest is therefore 5: 3 that answered plus 2 that didn't.
```

**Example 2**

```
Input: answers = [10,10,10]
Output: 11
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.Graphs;

/**
 * @author zhengxingxing
 * @date 2025/04/20
 */
public class RabbitsInForest {
    public static int numRabbits(int[] answers) {
        // Create an array to count responses (size 1000 due to constraint: answers[i] < 1000)
        // This array stores how many rabbits gave each specific answer
        int[] count = new int[1000];
        // Variable to store the minimum total number of rabbits
        int result = 0;

        // Iterate through each rabbit's answer
        for (int answer : answers) {
            /* Key Logic for grouping rabbits:
             * answer + 1 = size of each color group (including the rabbit itself)
             * count[answer] = number of rabbits processed so far who gave this answer
             *
             * The modulo operation (%) helps determine if we need a new group:
             * - When count[answer] % (answer + 1) == 0, it means all previous groups are full
             * - For example, if answer = 2:
             *   - Each group should have 3 rabbits
             *   - When count[2] = 0, 3, 6, etc., we need a new group
             */
            if (count[answer] % (answer + 1) == 0) {
                // Add a new complete group of rabbits
                // answer + 1 represents the total number of rabbits in this color group
                result += answer + 1;
            }

            // Increment the count for this answer
            // This tracks how many rabbits have given this specific answer
            count[answer]++;
        }

        return result;
    }
    
    public static void main(String[] args) {
        // Test Case 1: Mixed answers
        // Expected output: 5 (2 red rabbits + 3 blue rabbits)
        int[] test1 = {1, 1, 2};
        System.out.println("Test Case 1 Result: " + numRabbits(test1));

        // Test Case 2: Same answers
        // Expected output: 11 (one group of 11 rabbits)
        int[] test2 = {10, 10, 10};
        System.out.println("Test Case 2 Result: " + numRabbits(test2));

        // Test Case 3: Empty array
        // Expected output: 0 (no rabbits)
        int[] test3 = {};
        System.out.println("Test Case 3 Result: " + numRabbits(test3));

        // Test Case 4: Single answer
        // Expected output: 3 (one rabbit answered 2, so total 3 rabbits of same color)
        int[] test4 = {2};
        System.out.println("Test Case 4 Result: " + numRabbits(test4));
    }
}

```





