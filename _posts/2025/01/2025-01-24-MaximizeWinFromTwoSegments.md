---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "2555. Maximize Win From Two Segments"
date: "2025-01-24"
tags: Medium
categories:
  - "LeetCode DynamicSlidingWindow"
---



- There are some prizes on the **X-axis**. You are given an integer array `prizePositions` that is **sorted in non-decreasing order**, where `prizePositions[i]` is the position of the `ith` prize. There could be different prizes at the same position on the line. You are also given an integer `k`.
- You are allowed to select two segments with integer endpoints. The length of each segment must be `k`. You will collect all prizes whose position falls within at least one of the two selected segments (including the endpoints of the segments). The two selected segments may intersect.
  - For example if `k = 2`, you can choose segments `[1, 3]` and `[2, 4]`, and you will win any prize i that satisfies `1 <= prizePositions[i] <= 3` or `2 <= prizePositions[i] <= 4`.
- Return *the **maximum** number of prizes you can win if you choose the two segments optimally*.

**Example 1**

```
Input: prizePositions = [1,1,2,2,3,3,5], k = 2
Output: 7
Explanation: In this example, you can win all 7 prizes by selecting two segments [1, 3] and [3, 5].
```

**Example 2**

```
Input: prizePositions = [1,2,3,4], k = 0
Output: 2
Explanation: For this example, one choice for the segments is [3, 3] and [4, 4], and you will be able to get 2 prizes. 
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.SlideWindow.DynamicSlidingWindow;

/**
 * @author zhengxingxing
 * @date 2025/01/24
 */
public class MaximizeWinFromTwoSegments {
    public static int maximizeWin(int[] prizePositions, int k) {
        int n = prizePositions.length;
        // dp[i] represents the maximum number of prizes that can be collected
        // from the first `i` positions (0 to i-1) using one segment.
        int[] dp = new int[n + 1];
        // ans stores the final result, which is the maximum number of prizes
        // that can be collected using two segments.
        int ans = 0;
        // j is the left pointer of the sliding window, representing the start
        // of the current segment.
        int j = 0;

        // Traverse the array with the right pointer `i`.
        for (int i = 0; i < n; i++) {
            // Move the left pointer `j` to ensure the length of the current segment
            // [j, i] does not exceed `k`. If the length exceeds `k`, move `j` to the right.
            while (prizePositions[i] - prizePositions[j] > k) {
                j++;
            }
            // Update dp[i + 1], which represents the maximum number of prizes
            // that can be collected from the first `i + 1` positions (0 to i).
            // There are two choices:
            // 1. Do not select the current segment, and use the previous result dp[i].
            // 2. Select the current segment [j, i], which contains `i - j + 1` prizes.
            // We take the maximum of these two choices.
            dp[i + 1] = Math.max(dp[i], i - j + 1);
            // Update the final result `ans`. The total number of prizes is the sum of:
            // 1. The number of prizes in the current segment [j, i], which is `i - j + 1`.
            // 2. The maximum number of prizes that can be collected before position `j`,
            //    which is dp[j].
            ans = Math.max(ans, i - j + 1 + dp[j]);
        }

        // Return the final result.
        return ans;
    }

    public static void main(String[] args) {
        // Example 1
        int[] prizePositions1 = {1, 1, 2, 2, 3, 3, 5};
        int k1 = 2;
        System.out.println(maximizeWin(prizePositions1, k1)); // Output: 7

        // Example 2
        int[] prizePositions2 = {1, 2, 3, 4};
        int k2 = 0;
        System.out.println(maximizeWin(prizePositions2, k2)); // Output: 2
    }
}

```





