---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "986. Interval List Intersections"
date: "2024-11-17"
categories:
  - "LeetCode Two Pointers"
---

- You are given two lists of closed intervals, `firstList` and `secondList`, where `firstList[i] = [starti, endi]` and `secondList[j] = [startj, endj]`. Each list of intervals is pairwise **disjoint** and in **sorted order**.
- Return *the intersection of these two interval lists*.
- A **closed interval** `[a, b]` (with `a <= b`) denotes the set of real numbers `x` with `a <= x <= b`.
- The **intersection** of two closed intervals is a set of real numbers that are either empty or represented as a closed interval. For example, the intersection of `[1, 3]` and `[2, 4]` is `[2, 3]`.

**Example 1**

```
Input: firstList = [ [0,2],[5,10],[13,23],[24,25] ], secondList = [ [1,5],[8,12],[15,24],[25,26] ]
Output: [ [1,2],[5,5],[8,10],[15,23],[24,24],[25,25] ]
```

**Example 2**

```
Input: firstList = [ [1,3],[5,9] ], secondList = []
Output: []
```

## Method 1

```tex
【O(n + m) time | O(n + m) space】
```

```java
package Leetcode.TwoPointer;

import java.util.ArrayList;
import java.util.List;

/**
 * @author zhengxingxing
 * @date 2024/11/17
 */
public class IntervalListIntersections {
    public static int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        // ArrayList to store intersection intervals
        List<int[]> result = new ArrayList<>();
        // Two pointers to traverse both lists
        int i = 0;  // pointer for firstList
        int j = 0;  // pointer for secondList

        // Continue until we reach the end of either list
        while (i < firstList.length && j < secondList.length) {
            // Extract start and end points of current intervals
            int start1 = firstList[i][0];   // start of interval from first list
            int end1 = firstList[i][1];     // end of interval from first list

            int start2 = secondList[j][0];   // start of interval from second list
            int end2 = secondList[j][1];     // end of interval from second list

            // Find the intersection points
            // Intersection start is the maximum of both starts
            int intersectStart = Math.max(start1, start2);
            // Intersection end is the minimum of both ends
            int intersectEnd = Math.min(end1, end2);

            // If we have a valid intersection (start <= end)
            // add it to our result list
            if (intersectStart <= intersectEnd) {
                result.add(new int[]{intersectStart, intersectEnd});
            }

            // Move the pointer of the interval that ends first
            // This is crucial for not missing any potential intersections
            if (end1 < end2) {
                i++;  // first interval ends earlier, move to next interval in first list
            } else {
                j++;  // second interval ends earlier or at same time, move to next interval in second list
            }
        }

        // Convert ArrayList to array and return
        return result.toArray(new int[result.size()][]);
    }

    private static void printArray(int[][] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print("[" + arr[i][0] + "," + arr[i][1] + "]");
            if (i < arr.length - 1) {
                System.out.print(", ");
            }
        }
        System.out.println("]");
    }

    public static void main(String[] args) {
        // Test Case 1: Multiple intersections
        int[][] firstList1 = { { 0,2}, {5,10}, {13,23}, {24,25 } };
        int[][] secondList1 = { { 1,5}, {8,12}, {15,24}, {25,26 } };
        System.out.println("Test Case 1:");
        System.out.print("firstList = ");
        printArray(firstList1);
        System.out.print("secondList = ");
        printArray(secondList1);
        System.out.print("Intersection Result = ");
        printArray(intervalIntersection(firstList1, secondList1));
        System.out.println();

        // Test Case 2: Empty list scenario
        int[][] firstList2 = { { 1,3}, {5,9 } };
        int[][] secondList2 = {};
        System.out.println("Test Case 2:");
        System.out.print("firstList = ");
        printArray(firstList2);
        System.out.print("secondList = ");
        printArray(secondList2);
        System.out.print("Intersection Result = ");
        printArray(intervalIntersection(firstList2, secondList2));
        System.out.println();

        // Test Case 3: Complete overlap scenario
        int[][] firstList3 = { { 1,4}, {5,7 } };
        int[][] secondList3 = { { 1,4}, {5,7 } };
        System.out.println("Test Case 3 (Complete Overlap):");
        System.out.print("firstList = ");
        printArray(firstList3);
        System.out.print("secondList = ");
        printArray(secondList3);
        System.out.print("Intersection Result = ");
        printArray(intervalIntersection(firstList3, secondList3));
    }
}
```