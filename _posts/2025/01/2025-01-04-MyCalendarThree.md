---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "732. My Calendar III"
date: "2025-01-04"
tags: Hard
categories:
  - "LeetCode Trees"
---


- A `k`-booking happens when `k` events have some non-empty intersection (i.e., there is some time that is common to all `k` events.)
- You are given some events `[startTime, endTime)`, after each given event, return an integer `k` representing the maximum `k`-booking between all the previous events.
- Implement the `MyCalendarThree` class:
  - `MyCalendarThree()` Initializes the object.
  - `int book(int startTime, int endTime)` Returns an integer `k` representing the largest integer such that there exists a `k`-booking in the calendar.

**Example 1**

```
Input
["MyCalendarThree", "book", "book", "book", "book", "book", "book"]
[[], [10, 20], [50, 60], [10, 40], [5, 15], [5, 10], [25, 55]]
Output
[null, 1, 1, 2, 3, 3, 3]

Explanation
MyCalendarThree myCalendarThree = new MyCalendarThree();
myCalendarThree.book(10, 20); // return 1
myCalendarThree.book(50, 60); // return 1
myCalendarThree.book(10, 40); // return 2
myCalendarThree.book(5, 15); // return 3
myCalendarThree.book(5, 10); // return 3
myCalendarThree.book(25, 55); // return 3
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Trees;

import java.util.TreeMap;

/**
 * @author zhengxingxing
 * @date 2025/01/02
 */
class MyCalendarThree {
    // TreeMap to store time points and their corresponding event changes
    private TreeMap<Integer, Integer> timeline;

    /**
     * Initialize the calendar booking system
     */
    public MyCalendarThree() {
        timeline = new TreeMap<>();
    }

    /**
     * Books an event and returns the maximum number of overlapping events
     * @param startTime start time of the event
     * @param endTime end time of the event
     * @return maximum number of overlapping events (k-booking)
     */
    public int book(int startTime, int endTime) {
        // Increment booking count at start time
        timeline.put(startTime, timeline.getOrDefault(startTime, 0) + 1);
        // Decrement booking count at end time
        timeline.put(endTime, timeline.getOrDefault(endTime, 0) - 1);

        // Track current number of ongoing events
        int ongoing = 0;
        // Track maximum overlapping events seen so far
        int k = 0;

        // Iterate through the sorted time points
        for (int change : timeline.values()) {
            ongoing += change;
            k = Math.max(k, ongoing);
        }
        return k;
    }

    public static void main(String[] args) {
        // Test cases
        MyCalendarThree myCalendarThree = new MyCalendarThree();
        System.out.println("Expected: 1, Actual: " + myCalendarThree.book(10, 20));
        System.out.println("Expected: 1, Actual: " + myCalendarThree.book(50, 60));
        System.out.println("Expected: 2, Actual: " + myCalendarThree.book(10, 40));
        System.out.println("Expected: 3, Actual: " + myCalendarThree.book(5, 15));
        System.out.println("Expected: 3, Actual: " + myCalendarThree.book(5, 10));
        System.out.println("Expected: 3, Actual: " + myCalendarThree.book(25, 55));

        // Additional test cases
        MyCalendarThree calendar2 = new MyCalendarThree();
        System.out.println("\nAdditional test cases:");
        System.out.println(calendar2.book(0, 10));  // Returns 1
        System.out.println(calendar2.book(0, 10));  // Returns 2
        System.out.println(calendar2.book(0, 10));  // Returns 3
    }
}
```





