---
toc:
beginning: true
giscus_comments: true
layout: post
title: "1353. Maximum Number of Events That Can Be Attended"
date: "2025-07-07"
tags: Medium
categories:
- "LeetCode Greedy"
---

- You are given an array of `events` where `events[i] = [startDayi, endDayi]`. Every event `i` starts at `startDayi` and ends at `endDayi`.
- You can attend an event `i` at any day `d` where `startTimei <= d <= endTimei`. You can only attend one event at any time `d`.
- Return *the maximum number of events you can attend*.

**Example 1**

```
Input: events = [ [ 1,2],[2,3],[3,4 ] ]
Output: 3
Explanation: You can attend all the three events.
One way to attend them all is as shown.
Attend the first event on day 1.
Attend the second event on day 2.
Attend the third event on day 3.
```

**Example 2**

```
Input: events= [ [ 1,2],[2,3],[3,4],[1,2 ] ]
Output: 4
```

## Method 1

```tex
【O(U+nlogn) time | O(U) space】
```

```java
package Greedy;

import java.util.Arrays;

/**
 * @Author zhengxingxing
 * @Date 2025/07/07
 */
public class FindingPairsWithACertainSum {

    public static int maxEvents(int[][] events) {
        // Step 1: Sort all events by their end day in ascending order.
        // Greedy strategy: Always try to attend the event that ends earliest.
        Arrays.sort(events, (a, b) -> a[1] - b[1]);

        // Step 2: Find the latest end day among all events.
        int mx = events[events.length - 1][1];

        // Step 3: Initialize the Disjoint Set Union (DSU) parent array.
        // fa[x] represents the earliest available day >= x.
        // The array size is mx + 2 to avoid out-of-bounds when marking the last day as used.
        int[] fa = new int[mx + 2];
        for (int i = 0; i < fa.length; i++) {
            fa[i] = i; // Initially, each day is its own parent (available).
        }

        int ans = 0; // Counter for the maximum number of events attended.

        // Step 4: Iterate through each event and try to schedule it.
        for (int[] e : events) {
            // Use DSU to find the earliest available day to attend this event,
            // starting from its start day.
            int x = find(e[0], fa);

            // If the found day is within the event's interval, attend the event.
            if (x <= e[1]) {
                ans++; // Attend this event.

                // Mark this day as used by setting its parent to the next day.
                // This means the next search will skip this day.
                fa[x] = x + 1;
            }
            // If x > e[1], it means there is no available day for this event.
        }
        return ans;
    }
    
    private static int find(int x, int[] fa) {
        if (fa[x] != x) {
            // Recursively find the root parent and compress the path.
            fa[x] = find(fa[x], fa);
        }
        return fa[x];
    }

    // Test cases to verify the implementation.
    public static void main(String[] args) {
        int[][] events1 = { { 1,2},{2,3},{3,4 } };
        int[][] events2 = { { 1,2},{2,3},{3,4},{1,2 } };

        System.out.println(maxEvents(events1)); // Output: 3
        System.out.println(maxEvents(events2)); // Output: 4
    }
}

```





