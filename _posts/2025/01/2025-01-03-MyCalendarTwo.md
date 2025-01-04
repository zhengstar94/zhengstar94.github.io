---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "731. My Calendar II"
date: "2025-01-03"
tags: Medium
categories:
  - "LeetCode Trees"
---



- You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a **triple booking**.
- A **triple booking** happens when three events have some non-empty intersection (n.横断; 交叉;十字路口) (i.e., some moment is common to all the three events.).
- The event can be represented as a pair of integers `startTime` and `endTime` that represents a booking on the half-open interval `[startTime, endTime)`, the range of real numbers `x` such that `startTime <= x < endTime`.
- Implement the `MyCalendarTwo` class:
  - `MyCalendarTwo()` Initializes the calendar object.
  - `boolean book(int startTime, int endTime)` Returns `true` if the event can be added to the calendar successfully without causing a **triple booking**. Otherwise, return `false` and do not add the event to the calendar.


**Example 1**

```
Input
["MyCalendarTwo", "book", "book", "book", "book", "book", "book"]
[ [ ], [10, 20], [50, 60], [10, 40], [5, 15], [5, 10], [25, 55 ] ]
Output
[null, true, true, true, false, true, true]

Explanation
MyCalendarTwo myCalendarTwo = new MyCalendarTwo();
myCalendarTwo.book(10, 20); // return True, The event can be booked. 
myCalendarTwo.book(50, 60); // return True, The event can be booked. 
myCalendarTwo.book(10, 40); // return True, The event can be double booked. 
myCalendarTwo.book(5, 15);  // return False, The event cannot be booked, because it would result in a triple booking.
myCalendarTwo.book(5, 10); // return True, The event can be booked, as it does not use time 10 which is already double booked.
myCalendarTwo.book(25, 55); // return True, The event can be booked, as the time in [25, 40) will be double booked with the third event, the time [40, 50) will be single booked, and the time [50, 55) will be double booked with the second event.
```

## Method 1

```tex
【O(n) time | O(n) space】
```

```java
package Leetcode.Trees;

import java.util.Map;
import java.util.TreeMap;

/**
 * @author zhengxingxing
 * @date 2025/01/02
 */
class MyCalendarTwo {
    private TreeMap<Integer, Integer> delta;

    public MyCalendarTwo() {
        delta = new TreeMap<>();
    }

    public boolean book(int startTime, int endTime) {
        // Increment booking count at start time
        delta.put(startTime, delta.getOrDefault(startTime, 0) + 1);
        // Decrement booking count at end time
        delta.put(endTime, delta.getOrDefault(endTime, 0) - 1);

        int active = 0; // Track the number of active bookings at current time point

        // Check if this would cause a triple booking
        for (int d : delta.values()) {
            active += d;
            if (active >= 3) {
                // If triple booking would occur, rollback the operation
                delta.put(startTime, delta.get(startTime) - 1);
                if (delta.get(startTime) == 0) {
                    delta.remove(startTime);
                }
                delta.put(endTime, delta.get(endTime) + 1);
                if (delta.get(endTime) == 0) {
                    delta.remove(endTime);
                }
                return false;
            }
        }
        return true;
    }

    private void printTreeMapState() {
        System.out.println("\nCurrent TreeMap State:");
        for (Map.Entry<Integer, Integer> entry : delta.entrySet()) {
            System.out.printf("Time point: %d -> Change value: %d\n", entry.getKey(), entry.getValue());
        }
        System.out.println();
    }

    public static void main(String[] args) {
        // Create calendar object
        MyCalendarTwo calendar = new MyCalendarTwo();

        // Test case 1: Examples from the problem
        System.out.println("=== Test Case 1 ===");
        System.out.println("Book [10, 20]: " + calendar.book(10, 20));
        calendar.printTreeMapState();

        System.out.println("Book [50, 60]: " + calendar.book(50, 60));
        calendar.printTreeMapState();

        System.out.println("Book [10, 40]: " + calendar.book(10, 40));
        calendar.printTreeMapState();

        System.out.println("Book [5, 15]: " + calendar.book(5, 15));
        calendar.printTreeMapState();

        System.out.println("Book [5, 10]: " + calendar.book(5, 10));
        calendar.printTreeMapState();

        System.out.println("Book [25, 55]: " + calendar.book(25, 55));
        calendar.printTreeMapState();

        // Test case 2: Edge cases
        System.out.println("\n=== Test Case 2 ===");
        MyCalendarTwo calendar2 = new MyCalendarTwo();

        System.out.println("Book [0, 1]: " + calendar2.book(0, 1));
        System.out.println("Book [0, 1]: " + calendar2.book(0, 1));
        System.out.println("Book [0, 1]: " + calendar2.book(0, 1));  // Should return false

        // Test case 3: Non-overlapping scenarios
        System.out.println("\n=== Test Case 3 ===");
        MyCalendarTwo calendar3 = new MyCalendarTwo();

        System.out.println("Book [0, 10]: " + calendar3.book(0, 10));
        System.out.println("Book [20, 30]: " + calendar3.book(20, 30));
        System.out.println("Book [40, 50]: " + calendar3.book(40, 50));
    }
}
```





