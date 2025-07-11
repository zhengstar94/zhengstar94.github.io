---
toc:
beginning: true
giscus_comments: true
layout: post
title: "3169. Count Days Without Meetings"
date: "2025-07-11"
tags: Medium
categories:
- "LeetCode Math"
---


- You are given a positive integer `days` representing the total number of days an employee is available for work (starting from day 1). You are also given a 2D array `meetings` of size `n` where, `meetings[i] = [start_i, end_i]` represents the starting and ending days of meeting `i` (inclusive).
- Return the count of days when the employee is available for work but no meetings are scheduled.
- **Note:** The meetings may overlap.

**Example 1**

```
Input: days = 10, meetings = [ [ 5,7],[1,3],[9,10 ] ]

Output: 2

Explanation:

There is no meeting scheduled on the 4th and 8th days.
```

**Example 2**

```
Input: days = 5, meetings = [ [ 2,4],[1,3 ] ]

Output: 1

Explanation:

There is no meeting scheduled on the 5th day.
```

**Example 3**

```
Input: days = 6, meetings = [ [ 1,6 ] ]

Output: 0

Explanation:

Meetings are scheduled for all working days.
```

## Method 1

```tex
【O(nlogn) time | O(1) space】
```

```java
package Leetcode.Math;

import java.util.Arrays;

/**
 * @Author zhengxingxing
 * @Date 2025/07/11
 */
public class CountDaysWithoutMeetings {

    public static int countDays(int days, int[][] meetings) {
        // Sort the meetings by their start day in ascending order.
        Arrays.sort(meetings, (p, q) -> p[0] - q[0]);

        // 'start' and 'end' represent the current merged meeting interval.
        // Initialize 'start' to 1 (the first day), and 'end' to 0 (no interval yet).
        int start = 1;
        int end = 0;

        // Iterate through each meeting interval.
        for (int[] p : meetings) {
            // If the current meeting starts after the end of the last merged interval,
            // it means there is a gap (no meeting) between intervals.
            if (p[0] > end){
                // Subtract the number of days covered by the previous merged interval from 'days'.
                // (end - start + 1) is the length of the previous interval.
                days -= end - start + 1;
                // Start a new merged interval from the current meeting's start day.
                start = p[0];
            }
            // Extend the end of the current merged interval if needed.
            end = Math.max(end, p[1]);
        }
        // After the loop, subtract the days covered by the last merged interval.
        days -= end - start + 1;
        // The remaining 'days' is the number of days without any meetings.
        return days;
    }

    public static void main(String[] args) {
        // Test case 1
        int days1 = 10;
        int[][] meetings1 = { { 5,7},{1,3},{9,10 } };
        System.out.println(countDays(days1, meetings1)); // Output: 2

        // Test case 2
        int days2 = 5;
        int[][] meetings2 = { { 2,4},{1,3 } };
        System.out.println(countDays(days2, meetings2)); // Output: 1

        // Test case 3
        int days3 = 6;
        int[][] meetings3 = { { 1,6 } };
        System.out.println(countDays(days3, meetings3)); // Output: 0
    }
}

```





