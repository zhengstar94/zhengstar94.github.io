---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1423. Maximum Points You Can Obtain from Cards"
date: "2024-12-28"
tags: Medium
categories:
  - "LeetCode SlideWindow"
---

- There are several cards **arranged in a row**, and each card has an associated number of points. The points are given in the integer array `cardPoints`.
- In one step, you can take one card from the beginning or from the end of the row. You have to take exactly `k` cards.
- Your score is the sum of the points of the cards you have taken.
- Given the integer array `cardPoints` and the integer `k`, return the *maximum score* you can obtain.

**Example 1**

```
Input: cardPoints = [1,2,3,4,5,6,1], k = 3
Output: 12
Explanation: After the first step, your score will always be 1. However, choosing the rightmost card first will maximize your total score. The optimal strategy is to take the three cards on the right, giving a final score of 1 + 6 + 5 = 12.
```

**Example 2**

```
Input: cardPoints = [2,2,2], k = 2
Output: 4
Explanation: Regardless of which two cards you take, your score will always be 4.
```

**Example 3**

```
Input: cardPoints = [9,7,7,9,7,7,9], k = 7
Output: 55
Explanation: You have to take all the cards. Your score is the sum of points of all cards.
```

## Method 1

```tex
【O(n) time | O(1) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @author zhengxingxing
 * @date 2024/12/28
 */
public class MaximumPointsYouCanObtainFromCards {
    public static int maxScore(int[] cardPoints, int k) {
        int n = cardPoints.length; // Total number of cards.

        // Calculate the total points of all cards.
        int total = 0;
        for (int point : cardPoints) {
            total += point;
        }

        // If `k` equals the total number of cards, return the total sum of the array.
        if (k == n) {
            return total;
        }

        // Determine the size of the window to exclude from the selection.
        // This window contains the cards that are *not* part of the chosen `k` cards.
        int windowSize = n - k;

        // Calculate the initial sum of the first `windowSize` cards.
        int windowSum = 0;
        for (int i = 0; i < windowSize; i++) {
            windowSum += cardPoints[i];
        }

        // Initialize the minimum window sum to the sum of the first `windowSize` cards.
        int minWindowSum = windowSum;

        // Slide the window across the array, updating the window sum and tracking the minimum.
        for (int i = windowSize; i < n; i++) {
            // Add the next card in the window and remove the first card of the previous window.
            windowSum = windowSum + cardPoints[i] - cardPoints[i - windowSize];

            // Update the minimum window sum if the current window sum is smaller.
            minWindowSum = Math.min(minWindowSum, windowSum);
        }

        // The maximum score is the total sum minus the smallest sum of the excluded window.
        return total - minWindowSum;
    }

    public static void main(String[] args) {
        // Test case 1
        int[] cardPoints1 = {1, 2, 3, 4, 5, 6, 1};
        int k1 = 3;
        System.out.println("Test Case 1 Result: " + maxScore(cardPoints1, k1)); // Expected: 12

        // Test case 2
        int[] cardPoints2 = {2, 2, 2};
        int k2 = 2;
        System.out.println("Test Case 2 Result: " + maxScore(cardPoints2, k2)); // Expected: 4

        // Test case 3
        int[] cardPoints3 = {9, 7, 7, 9, 7, 7, 9};
        int k3 = 7;
        System.out.println("Test Case 3 Result: " + maxScore(cardPoints3, k3)); // Expected: 55
    }
}

```





