---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "1705. Maximum Number of Eaten Apples"
date: "2024-12-24"
tags: Medium
categories:
  - "LeetCode Heap"
---


- There is a special kind of apple tree that grows apples every day for `n` days. On the `ith` day, the tree grows `apples[i]` apples that will rot after `days[i]` days, that is on day `i + days[i]` the apples will be rotten and cannot be eaten. On some days, the apple tree does not grow any apples, which are denoted by `apples[i] == 0` and `days[i] == 0`.
- You decided to eat **at most** one apple a day (to keep the doctors away). Note that you can keep eating after the first `n` days.
- Given two integer arrays `days` and `apples` of length `n`, return *the maximum number of apples you can eat.*

**Example 1**

```
Input: apples = [1,2,3,5,2], days = [3,2,1,4,2]
Output: 7
Explanation: You can eat 7 apples:
- On the first day, you eat an apple that grew on the first day.
- On the second day, you eat an apple that grew on the second day.
- On the third day, you eat an apple that grew on the second day. After this day, the apples that grew on the third day rot.
- On the fourth to the seventh days, you eat apples that grew on the fourth day.
```

**Example 2**

```
Input: apples = [3,0,0,0,0,2], days = [3,0,0,0,0,2]
Output: 5
Explanation: You can eat 5 apples:
- On the first to the third day you eat apples that grew on the first day.
- Do nothing on the fouth and fifth days.
- On the sixth and seventh days you eat apples that grew on the sixth day.
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.Heap;

import java.util.PriorityQueue;

/**
 * @author zhengxingxing
 * @date 2024/12/24
 */
public class MaximumNumberOfEatenApples {
    public static int eatenApples(int[] apples, int[] days) {
        // Length of the growing period
        int n = apples.length;
        // Counter for eaten apples
        int result = 0;

        // Priority Queue storing [rotting_date, remaining_apples]
        // Sorted by rotting_date to always eat apples that will rot the earliest
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> {
            return a[0] - b[0];  // Sort by rotting date in ascending order
        });

        // Process each day, including days after the growing period (i >= n)
        // Continue while either:
        // 1. Still within growing period (i < n) OR
        // 2. Still have apples in queue (!pq.isEmpty())
        for (int i = 0; i < n || !pq.isEmpty(); i++) {
            // Remove all rotten apples
            // Keep removing while:
            // 1. Queue is not empty AND
            // 2. The earliest rotting date <= current day
            while (!pq.isEmpty() && pq.peek()[0] <= i) {
                pq.poll();
            }

            // Add new apples if:
            // 1. Still in growing period (i < n) AND
            // 2. There are apples grown on this day (apples[i] != 0)
            if (i < n && apples[i] != 0) {
                // Add [rotting_date, apple_count] to queue
                // rotting_date = current_day + days_until_rot
                pq.add(new int[] { i + days[i], apples[i] } );
            }

            // Try to eat an apple if any are available
            if (!pq.isEmpty()) {
                result++;  // Eat one apple
                // Decrease the count of apples in the current batch
                // If no apples left in this batch, remove it from queue
                if (--pq.peek()[1] == 0) {
                    pq.poll();
                }
            }
        }
        return result;
    }

    public static void main(String[] args) {
        // Test Case 1: Regular case
        int[] apples1 = {1, 2, 3, 5, 2};
        int[] days1 = {3, 2, 1, 4, 2};
        System.out.println("Test Case 1 Expected: 7, Actual: " + eatenApples(apples1, days1));

        // Test Case 2: Some days without apples
        int[] apples2 = {3, 0, 0, 0, 0, 2};
        int[] days2 = {3, 0, 0, 0, 0, 2};
        System.out.println("Test Case 2 Expected: 5, Actual: " + eatenApples(apples2, days2));

        // Test Case 3: Single day case
        int[] apples3 = {2};
        int[] days3 = {2};
        System.out.println("Test Case 3 Expected: 2, Actual: " + eatenApples(apples3, days3));

        // Test Case 4: Empty case
        int[] apples4 = {};
        int[] days4 = {};
        System.out.println("Test Case 4 Expected: 0, Actual: " + eatenApples(apples4, days4));

        // Test Case 5: All apples rot immediately
        int[] apples5 = {1, 2, 3, 4};
        int[] days5 = {0, 0, 0, 0};
        System.out.println("Test Case 5 Expected: 0, Actual: " + eatenApples(apples5, days5));
    }
}

```





