---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "252.Meeting Rooms"
date: "2024-06-15"
categories:
  - "LeetCode Intervals"
---

# 252. Meeting Rooms

- Given an array of meeting time intervals consisting of start and end times`[[s1,e1],[s2,e2],...]`(si< ei), determine if a person could attend all meetings.

**Example 1**

```
Input:
[[0,30],[5,10],[15,20]]
Output:
 false
```

**Example 2**

```
Input:
 [[7,10],[2,4]]

Output:
 true
```

## Method 1

```tex
【O(nlog(n)) time | O(1) space】
```

```java
package Leetcode.Intervals;

import java.util.Arrays;

/**
 * @author zhengstars
 * @date 2024/06/15
 */
public class MeetingRooms {
    /**
     * Method to check if a person can attend all given meetings without any overlap.
     *
     * @param intervals 2D array where each sub-array contains two integers denoting the start and end time of a meeting.
     * @return boolean value - true if a person can attend all meetings, false otherwise.
     */
    public static boolean canAttendMeetings(int[][] intervals) {
        // Sort the meeting intervals based on their start time.
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

        // Iterate through the sorted intervals to check for any overlaps.
        for (int i = 1; i < intervals.length; i++) {
            // If the start time of the current meeting is less than the end time of the previous meeting,
            // there is an overlap, and it's not possible to attend all meetings.
            if (intervals[i][0] < intervals[i - 1][1]) {
                return false; // Found an overlapping meeting, return false
            }
        }

        // If no overlaps found, return true meaning all meetings can be attended.
        return true;
    }

    public static void main(String[] args) {
        // Test case 1: Overlapping meetings
        int[][] intervals1 = { {0, 30}, {5, 10}, {15, 20} };
        System.out.println("Can attend all meetings for intervals1: " + canAttendMeetings(intervals1)); // Expected output: false

        // Test case 2: No overlapping meetings
        int[][] intervals2 = { {7, 10}, {2, 4} };
        System.out.println("Can attend all meetings for intervals2: " + canAttendMeetings(intervals2)); // Expected output: true
    }
}
```

