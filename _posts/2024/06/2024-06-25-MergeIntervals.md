---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "56.Merge Intervals"
date: "2024-06-25"
categories:
  - "LeetCode Intervals"
---

# 56. Merge Intervals

- Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return *an array of the non-overlapping intervals that cover all the intervals in the input*.

**Example 1**

```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```

**Example 2**

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

## Method 1

```tex
【O(nlogn) time | O(n) space】
```

```java
package Leetcode.Intervals;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * This class provides functionality to merge overlapping intervals.
 * Given a collection of intervals, it merges all overlapping intervals and
 * returns the result as a list of non-overlapping intervals.
 *
 * Author: zhengstars
 * Date: 2024/06/25
 */
public class MergeIntervals {
    /**
     * Merges overlapping intervals in the given array.
     *
     * @param intervals The array of intervals to merge.
     * @return A 2D array of merged intervals.
     */
    public static int[][] merge(int[][] intervals) {
        // Check for the special case where the intervals array is empty
        if(intervals.length == 0){
            return new int[0][0];
        }

        // Sort the intervals based on their start times
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        // List to store the merged intervals
        List<int[]> merged = new ArrayList<>();

        // Initialize the current interval to be merged
        int[] currentInterval = intervals[0];
        merged.add(currentInterval);

        // Iterate over the sorted intervals and merge where necessary
        for (int[] interval : intervals) {
            int currentEnd = currentInterval[1];

            // If the current interval overlaps with the next interval
            if(currentEnd >= interval[0]){
                // Update the end of the current interval to the maximum end time
                currentInterval[1] = Math.max(currentEnd, interval[1]);
            } else {
                // There is no overlap, so add the next interval as a new entry
                currentInterval = interval;
                merged.add(currentInterval);
            }
        }

        // Convert the list of merged intervals to a 2D array and return
        return merged.toArray(new int[merged.size()][]);
    }

    public static void main(String[] args) {
        // Example 1
        int[][] intervals1 = { {1, 3}, {2, 6}, {8, 10}, {15, 18} };
        System.out.println("Result for intervals1: ");
        printResult(merge(intervals1)); // Expected output: [[1, 6], [8, 10], [15, 18]]

        // Example 2
        int[][] intervals2 = { {1, 4}, {4, 5} };
        System.out.println("Result for intervals2: ");
        printResult(merge(intervals2)); // Expected output: [[1, 5]]
    }

    /**
     * Helper method to print a 2D array of intervals.
     *
     * @param intervals The array of intervals to print.
     */
    private static void printResult(int[][] intervals) {
        for (int[] interval : intervals) {
            System.out.print("[" + interval[0] + ", " + interval[1] + "] ");
        }
        System.out.println();
    }
}
```

