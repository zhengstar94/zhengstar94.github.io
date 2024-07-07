---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "253.Meeting Rooms II"
date: "2024-07-07"
categories:
  - "LeetCode Intervals"
---

# 253. Meeting Rooms II

- Given an array of meeting time intervals consisting of start and end times`[[s1,e1],[s2,e2],...]`(si< ei), find the minimum number of conference rooms required.

**Example 1**

```
Input:
[[0, 30],[5, 10],[15, 20]]
Output:
 2
```

**Example 2**

```
Input:
 [[7,10],[2,4]]

Output:
 1
```

## Method 1

```tex
【O(nlogn) time | O(n) space】
```

```java
package Leetcode.Intervals;

import java.util.Arrays;
import java.util.PriorityQueue;

/**
 * Given an array of meeting time intervals consisting of start and end times, find the minimum number of conference rooms required.
 *
 * @author zhengstars
 * @date 2024/07/07
 */
public class MeetingRoomsII {
    /**
     * Calculate the minimum number of conference rooms required for the given meeting time intervals.
     *
     * @param intervals an array of meeting time intervals
     * @return the minimum number of conference rooms required
     */
    public int minMeetingRooms(int[][] intervals) {
        if(intervals == null || intervals.length == 0){
            return 0;
        }

        // Sort the intervals by start time
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        minHeap.offer(intervals[0][1]);

        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] >= minHeap.peek()) {
                minHeap.poll();
            }
            minHeap.offer(intervals[i][1]);
        }
        return minHeap.size();
    }

    public static void main(String[] args) {
        MeetingRoomsII meetingRoomsII = new MeetingRoomsII();

        // Test Case 1
        int[][] intervals1 = { {0,30}, {5,10}, {15,20}};
        System.out.println("Test Case 1: " + meetingRoomsII.minMeetingRooms(intervals1)); // Output: 2

        // Test Case 2
        int[][] intervals2 = { {7,10}, {2,4}};
        System.out.println("Test Case 2: " + meetingRoomsII.minMeetingRooms(intervals2)); // Output: 1
    }
}
```

