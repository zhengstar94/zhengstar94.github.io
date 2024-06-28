---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "435.Non-overlapping Intervals"
date: "2024-06-28"
categories:
  - "LeetCode Intervals"
---

# 435. Non-overlapping Intervals

- Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return *the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping*.

**Example 1**

```
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
```

**Example 2**

```
Input: intervals = [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of the intervals non-overlapping.
```

**Example 3**

```
Input: intervals = [[1,2],[2,3]]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```

## Method 1

```tex
【O(nlogn) time | O(1) space】
```

```java
package Leetcode.Intervals;

import java.util.Arrays;

/**
 * Author: zhengstars
 * Date: 2024/06/28
 */
public class NonOverlappingIntervals {
    public static int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length == 0) {
            return 0;
        }

        // Sort the intervals by their ending times
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[1], b[1]));

        // Initialize the count of non-overlapping intervals
        int nonOverlapCount = 1;
        // Record the end time of the first interval
        int end = intervals[0][1];

        for (int i = 1; i < intervals.length; i++) {
            // If the start time of the current interval is greater than or equal to
            // the end time of the last recorded non-overlapping interval
            if (intervals[i][0] >= end) {
                nonOverlapCount++;
                // Update the end time to the end time of the current interval
                end = intervals[i][1];
            }
        }

        // The number of intervals to be removed is the total number of intervals
        // minus the count of non-overlapping intervals
        return intervals.length - nonOverlapCount;
    }

    public static void main(String[] args) {
        int[][] intervals1 = { {1, 2}, {2, 3}, {3, 4}, {1, 3} };
        System.out.println(eraseOverlapIntervals(intervals1)); // Output: 1

        int[][] intervals2 = { {1, 2}, {1, 2}, {1, 2} };
        System.out.println(eraseOverlapIntervals(intervals2)); // Output: 2

        int[][] intervals3 = { {1, 2}, {2, 3} };
        System.out.println(eraseOverlapIntervals(intervals3)); // Output: 0
    }
}
```

