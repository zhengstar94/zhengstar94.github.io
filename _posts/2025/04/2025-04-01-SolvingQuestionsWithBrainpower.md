---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2140. Solving Questions With Brainpower"
date: "2025-04-01"
tags: Medium
categories:
  - "LeetCode Dynamic Programming"
---


- You are given a **0-indexed** 2D integer array `questions` where `questions[i] = [pointsi, brainpoweri]`.
- The array describes the questions of an exam, where you have to process the questions **in order** (i.e., starting from question `0`) and make a decision whether to **solve** or **skip** each question. Solving question `i` will **earn** you `pointsi` points but you will be **unable** to solve each of the next `brainpoweri` questions. If you skip question `i`, you get to make the decision on the next question.
  - For example, given `questions = [[3, 2], [4, 3], [4, 4], [2, 5]]`:
    - If question `0` is solved, you will earn `3` points but you will be unable to solve questions `1` and `2`.
    - If instead, question `0` is skipped and question `1` is solved, you will earn `4` points but you will be unable to solve questions `2` and `3`.
- Return *the **maximum** points you can earn for the exam*.

**Example 1**

```
Input: questions = [[3,2],[4,3],[4,4],[2,5]]
Output: 5
Explanation: The maximum points can be earned by solving questions 0 and 3.
- Solve question 0: Earn 3 points, will be unable to solve the next 2 questions
- Unable to solve questions 1 and 2
- Solve question 3: Earn 2 points
Total points earned: 3 + 2 = 5. There is no other way to earn 5 or more points.
```

**Example 2**

```
Input: questions = [[1,1],[2,2],[3,3],[4,4],[5,5]]
Output: 7
Explanation: The maximum points can be earned by solving questions 1 and 4.
- Skip question 0
- Solve question 1: Earn 2 points, will be unable to solve the next 2 questions
- Unable to solve questions 2 and 3
- Solve question 4: Earn 5 points
Total points earned: 2 + 5 = 7. There is no other way to earn 7 or more points.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.DynamicProgramming;

/**
 * Author: zhengxingxing
 * Date: 2025/04/01
 */
public class SolvingQuestionsWithBrainpower {


    public static long mostPoints(int[][] questions) {
        // Total number of questions.
        int n = questions.length;

        // dp[i] stores the maximum points obtainable from question i to the end.
        // dp[n] is the base case (0 points) for when we are beyond the last question.
        long[] dp = new long[n + 1];

        // Iterate from the last question backwards.
        for (int i = n - 1; i >= 0; i--) {
            // Retrieve the points for the current question.
            int points = questions[i][0];
            // Retrieve the brainpower value (number of questions to skip after solving this question).
            int jump = questions[i][1];

            // Determine the next question index after solving the current one.
            // Ensure that nextPosition does not exceed the array bounds.
            int nextPosition = Math.min(n, i + jump + 1);

            // Two options at this question:
            // Option 1: Solve this question → points + dp[nextPosition]
            // Option 2: Skip this question → dp[i + 1]
            // Choose the maximum of the two.
            dp[i] = Math.max(points + dp[nextPosition], dp[i + 1]);
        }

        // dp[0] holds the maximum points obtainable starting from the first question.
        return dp[0];
    }


    public static void main(String[] args) {
        // Test Case 1:
        // Explanation: Optimal strategy is to solve question 0 (3 points) and question 3 (2 points),
        // resulting in a total of 5 points.
        int[][] questions1 = {{3, 2}, {4, 3}, {4, 4}, {2, 5}};
        System.out.println("Test Case 1 Result: " + mostPoints(questions1));  // Expected output: 5

        // Test Case 2:
        // Explanation: Optimal strategy is to skip question 0, solve question 1 (2 points)
        // and question 4 (5 points), resulting in a total of 7 points.
        int[][] questions2 = {{1, 1}, {2, 2}, {3, 3}, {4, 4}, {5, 5}};
        System.out.println("Test Case 2 Result: " + mostPoints(questions2));  // Expected output: 7
    }
}

```





