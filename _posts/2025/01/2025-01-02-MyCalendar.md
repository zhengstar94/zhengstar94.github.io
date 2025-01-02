---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "729. My Calendar I"
date: "2025-01-02"
tags: Medium
categories:
  - "LeetCode Trees"
---

- You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a **double booking**.
- A **double booking** happens when two events have some non-empty intersection (i.e., some moment is common to both events.).
- The event can be represented as a pair of integers `startTime` and `endTime` that represents a booking on the half-open interval `[startTime, endTime)`, the range of real numbers `x` such that `startTime <= x < endTime`.
- Implement the `MyCalendar` class:
  - `MyCalendar()` Initializes the calendar object.
  - `boolean book(int startTime, int endTime)` Returns `true` if the event can be added to the calendar successfully without causing a **double booking**. Otherwise, return `false` and do not add the event to the calendar.


**Example 1**

```
Input
["MyCalendar", "book", "book", "book"]
[[], [10, 20], [15, 25], [20, 30]]
Output
[null, true, false, true]

Explanation
MyCalendar myCalendar = new MyCalendar();
myCalendar.book(10, 20); // return True
myCalendar.book(15, 25); // return False, It can not be booked because time 15 is already booked by another event.
myCalendar.book(20, 30); // return True, The event can be booked, as the first event takes every time less than 20, but not including 20.
```

## Method 1

```tex
【O(log(n)) time | O(n) space】
```

```java
package Leetcode.Trees;

import java.util.TreeMap;

/**
 * @author zhengxingxing
 * @date 2025/01/02
 */
class MyCalendar {
    // Use TreeMap to store bookings, key is start time, value is end time
    private TreeMap<Integer, Integer> calendar;

    public MyCalendar() {
        calendar = new TreeMap<>();
    }

    public boolean book(int startTime, int endTime) {
        // Get the largest start time less than or equal to startTime
        Integer prevStart = calendar.floorKey(startTime);
        // Get the smallest start time greater than startTime
        Integer nextStart = calendar.ceilingKey(startTime);

        // Check if there's any conflict with the previous booking
        if (prevStart != null && calendar.get(prevStart) > startTime) {
            return false;
        }
        // Check if there's any conflict with the next booking
        if (nextStart != null && endTime > nextStart) {
            return false;
        }

        // No conflict found, add the new booking
        calendar.put(startTime, endTime);
        return true;
    }

    // Test cases in main method
    public static void main(String[] args) {
        MyCalendar myCalendar = new MyCalendar();

        // Test case 1: Basic booking scenarios
        System.out.println("Test case 1:");
        System.out.println(myCalendar.book(10, 20)); // Should return true
        System.out.println(myCalendar.book(15, 25)); // Should return false
        System.out.println(myCalendar.book(20, 30)); // Should return true

        // Test case 2: Edge cases
        System.out.println("\nTest case 2:");
        MyCalendar myCalendar2 = new MyCalendar();
        System.out.println(myCalendar2.book(0, 1));   // Should return true
        System.out.println(myCalendar2.book(1, 2));   // Should return true
        System.out.println(myCalendar2.book(0, 2));   // Should return false
    }
}

```





