---
toc:
beginning: true
giscus_comments: true
layout: post
title: "3439. Reschedule Meetings for Maximum Free Time I"
date: "2025-07-09"
tags: Medium
categories:
- "LeetCode SlideWindow"
---


- You are given an integer `eventTime` denoting the duration of an event, where the event occurs from time `t = 0` to time `t = eventTime`.

- You are also given two integer arrays `startTime` and `endTime`, each of length `n`. These represent the start and end time of `n` **non-overlapping** meetings, where the `ith` meeting occurs during the time `[startTime[i], endTime[i]]`.

- You can reschedule **at most** `k` meetings by moving their start time while maintaining the **same duration**, to **maximize** the **longest** *continuous period of free time* during the event.

- The **relative** order of all the meetings should stay the *same* and they should remain non-overlapping.

  Return the **maximum** amount of free time possible after rearranging the meetings.

  **Note** that the meetings can **not** be rescheduled to a time outside the event.

**Example 1**

```
Input: eventTime = 5, k = 1, startTime = [1,3], endTime = [2,5]
Output: 2

Explanation:

Reschedule the meeting at [1, 2] to [2, 3], leaving no meetings during the time [0, 2].
```

**Example 2**

```
Input: eventTime = 10, k = 1, startTime = [0,2,9], endTime = [1,4,10]

Output: 6

Explanation:

Reschedule the meeting at [2, 4] to [1, 3], leaving no meetings during the time [3, 9].
```

**Example 3**

```
Input: eventTime = 5, k = 2, startTime = [0,1,2,3,4], endTime = [1,2,3,4,5]

Output: 0

Explanation:

There is no time during the event not occupied by meetings.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.SlideWindow;

/**
 * @Author zhengxingxing
 * @Date 2025/07/09
 */
public class RescheduleMeetingsForMaximumFreeTimeI {

    public static int maxFreeTime(int eventTime, int k, int[] startTime, int[] endTime) {
        int n = startTime.length;
        // The gaps array stores all free time intervals between meetings and at the boundaries.
        // gaps[0]: Free time from the start of the event to the first meeting.
        // gaps[i]: Free time between the end of meeting i-1 and the start of meeting i.
        // gaps[n]: Free time from the end of the last meeting to the end of the event.
        int[] gaps = new int[n + 1];
        gaps[0] = startTime[0]; // Free time before the first meeting.
        for (int i = 1; i < n; ++i) {
            gaps[i] = startTime[i] - endTime[i - 1]; // Free time between consecutive meetings.
        }
        gaps[n] = eventTime - endTime[n - 1]; // Free time after the last meeting.

        // Use a sliding window of size k+1 to merge k+1 consecutive free intervals.
        int windowSum = 0;
        // Calculate the sum of the first window.
        for (int i = 0; i <= k; ++i) windowSum += gaps[i];
        int ans = windowSum;
        // Slide the window across the gaps array, updating the sum and tracking the maximum.
        for (int i = k + 1; i < gaps.length; ++i) {
            windowSum += gaps[i] - gaps[i - k - 1];
            ans = Math.max(ans, windowSum);
        }
        return ans;
    }
    
    public static void main(String[] args) {
        int eventTime = 10, k = 1;
        int[] startTime = {0, 2, 9};
        int[] endTime = {1, 4, 10};
        // Expected output: 6
        System.out.println(maxFreeTime(eventTime, k, startTime, endTime));
    }
}
```





