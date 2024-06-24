---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "57.Insert Interval"
date: "2024-06-16"
categories:
  - "LeetCode Intervals"
---

# 57. Insert Interval

- You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.
- Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).
- Return `intervals` *after the insertion*.
- **Note** that you don't need to modify `intervals` in-place. You can make a new array and return it.

**Example 1**

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2**

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Intervals;

import java.util.ArrayList;
import java.util.List;

/**
 * Author: zhengstars
 * Date: 2024/06/16
 */
public class InsertInterval {
    public static int[][] insert(int[][] intervals, int[] newInterval) {
        // List to store the final result after merging intervals
        List<int[]> result = new ArrayList<>();

        // Length of the input intervals array
        int n = intervals.length;

        // Index for iterating through the intervals array
        int i = 0;

        // Add all intervals that end before the new interval starts
        while (i < n && intervals[i][1] < newInterval[0]) {
            result.add(intervals[i]);  // These intervals do not overlap with the new interval
            i++;
        }

        // Merge all intervals that overlap with the new interval
        while (i < n && intervals[i][0] <= newInterval[1]) {
            // Update the new interval to encompass the overlapping intervals
            newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
            newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
            i++;
        }

        // Add the merged new interval to the result
        result.add(newInterval);

        // Add all remaining intervals that start after the new interval ends
        while (i < n) {
            result.add(intervals[i]);
            i++;
        }

        // Convert the result list to a two-dimensional array
        return result.toArray(new int[result.size()][]);
    }

    public static void main(String[] args) {
        // Test case 1
        int[][] intervals1 = { {1, 3}, {6, 9} };
        int[] newInterval1 = {2, 5};
        System.out.println("Result for intervals1: ");
        printResult(insert(intervals1, newInterval1)); // Expected output: [[1, 5], [6, 9]]

        // Test case 2
        int[][] intervals2 = { {1, 2}, {3, 5}, {6, 7}, {8, 10}, {12, 16} };
        int[] newInterval2 = {4, 8};
        System.out.println("Result for intervals2: ");
        printResult(insert(intervals2, newInterval2)); // Expected output: [[1, 2], [3, 10], [12, 16]]
    }

    /**
     * Helper method to print a two-dimensional array of intervals.
     *
     * @param intervals The array of intervals to be printed.
     */
    private static void printResult(int[][] intervals) {
        for (int[] interval : intervals) {
            System.out.print("[" + interval[0] + ", " + interval[1] + "] ");
        }
        System.out.println();
    }
}
```

